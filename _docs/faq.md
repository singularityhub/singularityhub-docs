---
title: Frequently Asked Questions
pdf: true
toc: false
---

# Frequently Asked Questions

 - [What is Singularity Hub](#what-is-singularity-hub)
 - [What is a Linux Container?](#what-is-a-linux-container)
 - [How is Singularity Hub related to Sylabs and Singularity](#how-is-singularity-hub-related-to-sylabs-and-singularity)
 - [Is Singularity Hub open source?](#is-singularity-hub-open-source)
 - [Why do we need Singularity containers?](#why-do-we-need-singularity-containers)
 - [How is Singularity Hub Different from Singularity Registry Server?](#how-is-singularity-hub-different-from-singularity-registry-server)
 - [Does Singularity Hub Test my Container?](#does-singularity-hub-test-my-containers)

### What is Singularity Hub?

Singularity Hub is a registry for scientific [linux containers](https://opensource.com/resources/what-are-linux-containers).

### What is a Linux Container?

A container image is an encapsulated, portable environment that is created to distribute a scientific analysis or a general function. Containers help with reproducibility of such content as they nicely package software and data dependencies, along with libraries that are needed. Thus, the core of Singularity Hub are these Singularity container images, and by way of being on Singularity Hub they can be easily built, updated, referenced with a url for a publication, and shared. This small guide will help you to get started building your containers using Singularity Hub and your Github repositories.

### How is Singularity Hub related to Sylabs and Singularity?

[Sylabs](https://sylabs.io) is the company that maintains the open source [Singularity](https://www.github.com/sylabs/singularity) code base, along with providing services for paying customers that use Singularity containers. The creator and maintainer of Singularity Hub was one of the original Singularity (open source software) developers, however she is not affiliated with the company Sylabs. Singularity Hub continues to be maintained by this individual at Stanford University, with generous support from Google Cloud. The two now co-exist peacefully, both passionate about using and supporting users to build Singularity containers. The distinction between Sylabs Library and Singularity Hub (and [Singularity Registry Server](https://www.github.com/singularityhub/sregistry)) comes down to the intended communities that are served. Singularity Hub and Registry is a non-enterprise solution that is catered for research scientists.

### Is Singularity Hub open source?

The source code for Singularity Hub is private, however the author has released an open source (somewhat) equivalent in the case that you want to deploy your own registry, [Singularity Registry Server](https://www.github.com/singularityhub/sregistry).

### Why do we need Singularity containers?

Singularity containers allow you to package your entire scientific analysis, including dependencies, libraries, and environment, and run it anywhere. Inside a Singularity container you are the same user as outside the container, so you could not escalate to root and act maliciously on a shared resource. For more information on containers, see [the Singularity site](https://singularityware.github.io).

### How is Singularity Hub Different from Singularity Registry Server?

**Singularity Hub**

is the predecessor to Singularity Registry, and while it also serves as an image registry, in addition it provides a cloud build service for users. Singularity Hub also takes advantage of Github for version control of build recipes. The user pushes to Github, a builder is deployed, and the image available to the user. Singularity Hub would allow a user to build and run an image from a resource where he or she doesn't have sudo simply by using Github as a middleman.

**Singularity Registry Server** 

is similarly an image registry that plugs in natively to the singularity software, but it places no dependencies on Github, and puts the power of deciding how to build in the hands of the user. This could mean building after tests pass with a "push" command in a Github repository, building via a SLURM job, or on a private server. While Singularity Hub is entirely public and only allows for a minimum number of private images, a Singularity Registry Server can be entirely private, with expiring tokens that can be shared. The administrator can choose
to include Singularity Hub like features via plugins, but only if they are desired.

Importantly, both deliver image manifests that plug seamlessly into the Singularity command-line software, so a registry (or hub) image can be pulled easily:

```bash
singularity pull shub://vsoch/hello-world
```

## Does Singularity Hub Test my Containers?

Singularity Hub will do a basic test to see if your container runs a command to list files. This test will return that the build is successful given a return code 0, and this is the approach that we must take to support a wide variety of containers with different operating systems inside.  If you want
further testing, we recommend that you do a [deployment build](builds/deployment) and test with your own
continuous integration.
