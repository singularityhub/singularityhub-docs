---
title: Building Containers
pdf: true
toc: false
permalink: docs/builds/
---

# Meet the Builders

> Hi there, I'm one of the Singularity Hub robots! I'm here to help build your containers.

<img style="width:30%" src="https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/robot_package.png">

## What is a builder?
A builder is an instance, deployed on demand when it's needed, on Google Cloud.

 - **Machine Type**: The machine type is a [n1-standard-2](https://cloud.google.com/compute/docs/machine-types#standard_machine_types). This is a rather small instance, with 2 virtual CPU and approximately 8 GB of memory.
 - **Size** The builder uses a 100GB instance.
 - **Singularity Version** The default Singularity version is the legacy 2.5.1, as version 2.x of Singularity is still in use by many clusters. We will also provide more recent versions of Singularity
 - **Operating System** The base OS is Ubuntu 18.04 with LTS.

The instance, for Singularity 2.0, comes with software and libraries essential for Singularity and the Singularity python software. We also install several modules (e.g., yum, deboostrap) for the build environment to address all the bugs that come with building certain images. Unfortunately Ubuntu 18.04 no longer offers zypper, and so it's not supposed. The build is done in an enclosed environment on the host, so you will not have any access to the host.

## How do I customize build settings?

To specify your builder and other settings discussed, you should edit a collection's [Build Settings](settings). 
Here you can customize the following options:

 * **Builder**: By default, we use an Ubuntu 16.04 image with debootstrap and yum installed, and Singularity 2.3. You can change that in this view. We currently provide the builders, and if you find none are suitable, please [submit an issue](https://github.com/singularityhub/singularityhub.github.io/issues/new) and we will figure out a solution right away! 
 * **Debug**: Set debug to true to make the output of the builder more verbose.

There are a few important variables that you can change the default settings for. When we update the builders on Singularity Hub (for example, when a new version is released) all new collections will use the latest builder, by default. However, collections that are already existing will not be updated automatically, as this may not be your preference. 

If you have different needs than our default builders, please [reach out](https://github.com/singularityhub/singularityhub.github.io/issues).

<br>

# Building Options

Singularity Hub supports the following types of builds

 - [Automated Build](automated): build containers automatically from Github commits
 - [Manual Build](manual): Only build when you issue the command.
 - [Deployment Build](deployment): Triggered only by a deployment from Github.

Each has different use cases, and you should read about each of the above to know what is best for you. For most users, the Automated Build is preferred, but if you want to do testing on your own first, you might choose Deployment Build. If you want to explicitly select commits and recipes and when to do it, or to go back in time to build a previous container, you will want to enable manual build. You are free to change your building selection at any time in the collection settings.

{% include alert.html title="Recommendation for Frequent Updates" content="If you push to your GitHub repository often, it's highly recommended that you enable manual or deployment builds. Every commit to master (and other branches you've enabled) will be added to a queue to build, and not only will you be wasting resources, if the commits are excessive your collection will be turned off for a 24 hour period." %}

In addition, you can read about the [build metadata](metadata) that is provided after the build completes.
