---
layout: post
title: Docker Labels
permalink: /dockerlabels/
---
Since Docker 1.6.0 there is a concept called ['labels'(https://github.com/docker/docker/pull/9882), which is pretty much exactly what I need for service discovery. As I have mentioned before, I'm (ab)using ENV-vars for this. That works fine, but that has the (probably undesired) side effect that the container itself has access to these variables.

So labels are definitely better, and I'll rewrite Tasman to utilize labels instead of ENV when there is a new release of Boot2docker. That should be reasonably trivial.
 
