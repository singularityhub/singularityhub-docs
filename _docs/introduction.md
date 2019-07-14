---
title: Introduction
pdf: true
toc: false
---

# Introduction

## A Need for Reproducible Science

Using computational methods to answer scientific questions of interest is an important task to increase our knowledge about the world. Along with careful assembly of protocol and relevant datasets, the scientist must also write software to perform the analysis, and use the software in combination with data to answer the question of interest. When a result of interest to the larger community is found, the scientist writes it up for publication in a scientific journal. This is what we might call a single scientific result.

Replication of a result would increase our confidence in the finding. The extent to which a published finding affords a second scientist to repeat the steps to achieve the result is called reproducibility. Reproducibility, in that it allows for repeated testing of an interesting question to validate knowledge about the world, is a foundation of science. While the original research can be an arduous task, often the culmination of years of work and committment, attemps to reproduce a series of methods to assess if the finding replicates is equally challenging. The researcher must minimally have enough documentation to describe the original data and detailed methods to put together an equivalent experiment. A comparable computational environment must then be used to look for evidence to assert or reject the original hypothesis.

Unfortunately, many scientists are not able to provide the minimum product to allow others to reproduce their work. It could be an issue of time - the modern scientist is burdened with writing grants, managing staff, and fighting for tenure. It could be an issue of education. Graduate school training is heavily focused on a particular domain of interest, and developing skills to learn to program, use version control, and test is outside the scope of the program. It might also be entirely infeasible. If the experiments were run on a particular supercomputer and/or with a custom software stack, it is a non trivial task to provide that environment to others. The inability to easily share environments and software serves as a direct threat to scientific reproducibility.



{% include alert.html title="Citation" type="info" content="Sochat V, Prybol CJ, Kurtzer GM (2017)<br> Enhancing reproducibility in scientific computing: Metrics and registry for Singularity containers.<br> PLoS ONE 12(11): e0188511. https://doi.org/10.1371/journal.pone.0188511" %}

## Encapsulation of Environments with Containers

The idea that the entire software stack, including libraries and specific versions of dependencies, could be put into a container and shared offered promise to help this problem. Linux containers, which can be thought of as encapsulated environments that host their own operating systems, software, and flie contents, were a deal breaker when coming onto the scene in early 2015. Like Virtual Machines, containers can make it easy to run a newer software stack on an older host, or to package up all the necessary software to run a scientific experiment, and have confidence that when sharing the container, it will run without a hitch. In early 2015, an early player on the scene, an enterprise container solution called Docker, started to be embraced by the scientific community. Docker containers were ideal for enterprise deployments, but posed huge security hazards if installed on a shared resource.

It wasn't until the introduction of the Singularity software that these workflows could be securely deployed on local cluster resources. For the first time, scientists could package up all of the software and libraries needed for their research, and deliver a complete package for a second scientist to reproduce the work. Singularity took the high performance computing world by storm, securing several awards and press releases, and within a year being installed at over 45 super computing centers across the globe. 


## Terms
Let's now talk about some commonly used terms.

### collections
Each container image (eg, `shub://fishman/snacks`) is actually a set of images called a `collection`.

<br>
Within a collection you might have different tags or versions for images. For example:

 - `milkshake/banana:pudding`
 - `milkshake/chocolate:pudding`
 - `milkshake/vanilla:pudding`

All of these images are derivations of `milkshake`, and so we find them in the same collection. I chose above to change the name of the container and maintain the same tag, you could of course have more granular detail, different versions of the same container:

 - `milkshake/banana:v1.0`
 - `milkshake/banana:v2.0`


### Labels

A `label` is an important piece of metadata (such as version, creator, or build variables) that is carried with a container to help understand it's generation, and organize it. When you create a Singularity image, you might bootstrap from Docker, and or add a "labels" section to your build definition:

```bash
%labels
MAINTAINER vanessasaur
```

### Topic Tags

Topic tags are a way for users to describe containers. Singularity Hub grabs topic tags
directly from the GitHub repository from where it was built.

### Favorites
Do you have a favorite collection? You can star it! Each Singularity Registry keeps track of the number of downloads (pull) of the containers, along with stars!
