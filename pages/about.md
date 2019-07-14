---
title: About
permalink: /about/
---

# About

Singularity Hub is a registry for Singularity containers. It was developed and
still is maintained by Stanford University, and is possibly by way of
funding from Google. To read more about it's background, see {% include doc.html name="the introduction" path="introduction" %}. For policy, including license, data and usage agreement, see [here]({{ site.baseurl }}/docs/regulatory). 

## How does it work?

Generally, you will follow these basic steps to build containers:

#### Create an Account

You create an account by authenticating with GitHub. This gives Singularity
Hub the ability to create webhooks that will will trigger a new build at each 
push to the repository. One container collection coincides with one repository, 
and holds multiple images.

### Push to GitHub
Push one or more Singularity build recipes (text files with a specification for your
build called `Singularity.*`) to the repository to trigger the build

### Builders
Upon receiving notification of your push, Singularity-Hub will launch a build.
The container is built in the cloud, and when finished, is programmatically
accessible from the Singularity command line client, or the [Singularity Global client](https://singularityhub.github.io/sregistry-cli). 


A few important notes for you:
 - By default, we only listen for changes to the master branch. To enable other branches, you need to add them in your Collection settings.
 - Please read about {% include doc.html name="build limits" path="build-limits" %} before creating your repositories, along with {% include doc.html name="naming images" path="naming-images" %}.

Singularity Hub is developed at Stanford University with support from Google Cloud. Thank you!

## Support

If you need help, please don't hesitate to [open an issue](https://www.github.com/singularityhub.github.io/issues).
