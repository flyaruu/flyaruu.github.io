---
layout: post
title: Service Discovery
permalink: /discovery/
---
Service Discovery is an old problem, but one that has gotten much more attention recently because of the popularity of microservices.

The ultimate goal is to have a mechanism where every service has a continuous and correct view of which other services are available so it can find and consume those services, and so it can respond accordingly if there is a changes. If, for example, a service you are using disappears, it probably makes sense to look for another compatible, service. If there isn't any and it is essential for your service, it might make sense to retract your own service, so other services depending on you can respond to that appropriately. In a way this is a pro-active version of the 'circuit-breaker' pattern, as it can prevent calls to broken services.

#### OSGi

This is pretty much how services work in OSGi, and it works well. (For a more in-depth explanation about OSGi, check (http://www.osgi.org/Technology/WhatIsOSGi))

The scope is limited though: The service discovery only focuses on:

 - One host
 - Only OSGi services

This was less of an issue in the early days (remember that the OSGi spec. is over 15 years old) but nowadays you really need to be able to cluster hosts and consume non-OSGi services.

#### Distributed OSGi
There is something called Distributed OSGi that can be of help. This allows OSGi instances to export their services to other instances so other instances can discover and consume these services transparently, in other words, the service consumer does not need to know that the service is remote. Personally I'm not convinced that this is the way to go. Granted, it is very convenient if all 'remoteness' is abstracted away, so you don't have to think about things like network transport, and if it works, well.. great, but it remains uncertain if it is generally possible and desirable to abstract this.

#### Micro services
Be it as it may, more and more services are not Java based, and definitely not OSGi based, and we should consider those too. In pretty much any system you will need some kind of database, maybe a mail server, or a third party HTTP service.

There has been (still is, I guess) some debate about if an OSGi service is a 'micro service', I don't want to get into this, but there is no denying that an OSGi-only has a drawback that there is no free choice of technology stack: It *has* to play nice with Java, and even then some Java code is notoriously hard to get to work well in an OSGi environment.

For these services it is not necessary to abstract that they are remote, as all the 'drivers' are network based to begin with, and will assume remoteness anyway. SQL databases use JDBC, mail is sent over SMTP. So in short we can integrate in a simpler way: We only need to 'discover' the configuration. We need to know to *which* JDBC url to connect, or to which SMTP server to connect, and we can leave the transport to the drivers.


