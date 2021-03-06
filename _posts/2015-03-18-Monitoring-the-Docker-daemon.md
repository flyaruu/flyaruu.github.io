---
title: Service Discovery using the Docker API
layout: post
permalink: /dockerapi/
---
Service Discovery using the Docker API

This part is loosely based on Jeff Lindsay's blog [here](http://progrium.com/blog/2014/09/10/automatic-docker-service-announcement-with-registrator/), it's a good read and does a better job at explaining than I do.

The premise is that you can query Docker hosts for their running services, and along with some meta data, expose it to a service discovery tool.

On any host running Docker containers there is a Docker daemon process. This Docker daemon offers a very nice API using simple JSON HTTP calls. All docker functions are exposed through this API (starting, stopping, building, etc.) but for service discovery I'll just use it as a 'read-only' source: We'll just use it to do discover what is running. Let's look at an example.

If I run the standard mysql docker container, I can run it like this:

{% highlight bash %}
docker run -e MYSQL_ROOT_PASSWORD=mysecretpassword -P mysql
{% endhighlight %}
The '-e' switch sets an environment variable, this particular one is required for mysql, and the '-P' switch lets docker publish this container's ports to a randomly chosen port on the host.

If I inspect that container using the HTTP/JSON API it looks like [this](/info_json/).

It's quite a blob of information and I won't go into all the details, but for now these parts are interesting:

{% highlight yaml %}
"Env" : [ "MYSQL_ROOT_PASSWORD=mysecretpassword",
   "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
   "MYSQL_MAJOR=5.6",
   "MYSQL_VERSION=5.6.23"
]
{% endhighlight %}

and especially:
{% highlight yaml %}
  "NetworkSettings" : {
    "Ports" : {
      "3306/tcp" : [ {
        "HostIp" : "0.0.0.0",
        "HostPort" : "49212"
      } ]
    }
{% endhighlight %}

Using this information I can sort of make out that this is, in fact, a mysql database, running on port 49212. 

For any kind of meaningful service discovery this is not enough: We basically now know there is *probably* some kind of mysql service running at 49212. At the very least we'd like to know for sure that this is indeed a mysql service, and also we'll need to tag or annotate it so we can publish which instance it is or any other info the consumer might need to know.

The good news is that we can easily add extra environment parameters to our call without needing to create a new image. So let's introduce a 'special' environment variable called SERVICE_NAME with the value 'mysql' and some SERVICE_TAGS for example with value "app1,test"

{% highlight bash %}
docker run -e MYSQL_ROOT_PASSWORD=mysecretpassword -e SERVICE_NAME=mysql -e SERVICE_TAGS=app1,test -P mysql
{% endhighlight %}

So now we have annotated this service in a very basic way: We've announced that this is indeed a mysql service, and that it is tagged with 'app1' and 'test', by (ab)using environment variables. [^1]

[^1]: From Docker 1.6.0 on, there will be 'labels', which is what we'd actually like to use here. The difference is that we only use the environment variables as service meta data, but the environment variables are (obviously) visible to the container. This is conceptually wrong, although I guess rarely an actual problem.



With this done we have the following data:
 - There is a service of type 'mysql'
 - It is running on the docker host's ip, on port 49212
 - It is tagged with 'app1' and 'test'

So what we are looking for now is that another service can express interest in a service (with certain properties) and our service discovery can find an appropriate service, as well as notify when instances come and go.

In the [next part](/OSGi/) we will talk about how to get this information into OSGi
