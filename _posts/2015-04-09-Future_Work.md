---
layout: post
title: Conclusions and Future work
permalink: /futurework/
---

#### Explore other configuration sources.
We've focused only on the docker daemon now, but other configuration sources might make sense too, like listening for services in a Kubernetes cluster

#### Improve performance

  - Use event mechanism instead of polling on the Docker API
  - Use long polling to Consul
   
#### Standardization 

   - A well-known schema on how to annotate docker containers


   
