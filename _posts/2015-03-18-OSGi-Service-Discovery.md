OSGi is (among other things) a Java dynamic service discovery tool.

On the lowest level, it works like this:

To announce a service, you register a service on the service registry:
```
ServiceRegistration<ServiceObject> registered = bundleContext.registerService(ServiceObject.class, pool, properties);
```
Any Java Object can be a service. The 'properties' dictionary is a map, of service properties which can be used by a service consumer to find and select services.

These services can now be queried or subscribed to. I won't go into the gritty details here, but on the lowest level there is an event bus that reports all services coming and going, and on a higher level you can declaratively state that you want to bind to a service with certain properties.

```
@Reference(target="(test=*)")
public void setDataSourceFactory(DataSourceFactory source) {
	doThings(source);
	// ... do stuff with the datasource
}
```
So here we express our interest in a DataSourceFactory object with tag 'test'. From the application's end, this is all we do, we rely on the OSGi and the service discovery to find us that object if it exists, or bind it on arrival if it does not. We don't care how it was found.

This works pretty well from the consumers point of view, we can't make it much easier than this, but this is still pretty unwieldy from the service provider's point of view.

Let's take a step back. In the previous page we had a mysql service with some properties:

 - It is running on the docker host's ip, let's say 127.0.0.1, on port 49212
 - It is tagged with 'app1' and 'test'

So we can now make a 'bridge' between the Docker JSON API and the OSGi service registry. We could instantiate an appropriate object (I guess a MySqlDataSourceFactory or something from the mysql Java driver) set all the right properties and we will have a working database connection.

It works, but it does create a problem: This approach requires the 'bridge' to know what MySQL is. It needs to have a MySQL driver and know how to use it.

This is unacceptable. There is no way the bridge can know which services there can be (or will be) and how to deal with them in Java. Also there might be different ways to interact with a service. For example, if I am using an ElasticSearch indexer, I could use a specific ElasticSearch driver, but as it simply speaks HTTP, a simple HTTP connection might be easier in some cases.

In sort: We will need to decouple this, and OSGi has the perfect tool for this: the Configuration Admin.

This standard service in OSGi introduces the notion of configuration. A service (remember: Can be any Java object) can be bound to a configuration object, which is basically a persistent id (or 'pid') and a general key value map.

Cutting some corners but in essence: A service can indicate that it needs configuration data and that it has a certain persistent id. When the appropriate configuration is available, the OSGi runtime will instantiate the service and associate it with that configuration.

```
@Component(name="docker.osgi.mysql", configurationPolicy=ConfigurationPolicy.REQUIRE, service={DataSourceFactory.class})
public class MySQLInstance implements DataSourceFactory {

	@Activate
	public void activate(Map<String,Object> properties) {
		String host = (String)properties.get("host.ip");
	    int port =  Integer.parseInt((String) properties.get("host.port"));
	    // initialize database connection
	    // .. do stuff
	}
...
}
```
In this piece of Java code we state that we are a component (=service) that *requires* configuration with pid "docker.osgi.mysql" and it will expose an DataSourceFactory interface to the service bus.

Note that we still need a 'driver' bundle for every type of service, but it now has a much simpler responsibility: It needs to listen for configuration with the service pid's it is interested in (in this case docker.osgi.mysql) and instantiate a service that makes sense

Note that there will be an instance for each available configuration object (there could be many configuration objects for a service. If a configuration object gets retracted, the service will be deactivated.

So this solves our problem: The only thing the bridge needs to do is:
 - Figure out the pid to use (At this point I use "docker.osgi.<SERVICE_NAME>)
 - Extract the properties from the environment variables
 - Create a configuration object

Note that the bridge does not know if or when some service will accept those configuration objects, nor does it care. The configuration object will happily float around in an unbound state until somebody is interested.

Finally the bridge has the resposibility to unregister the configuration objects when the service disappears. At this point that means that the container supplying the service is stopped. The configuration admin service will then stop any services bound to that configuration object.


