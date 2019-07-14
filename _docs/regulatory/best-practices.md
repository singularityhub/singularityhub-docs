---
title: Singularity Hub Documentation
---

# Best Practices

Given the [limits](limits) outlined previously, there are most definitely best
practices that we can provide to both use Singularity Hub and build containers.
Importantly, if you don't follow these best practices it can mean slowing down
your research if you exceed the Singularity Hub rate limits for API or other
views.


## Recipe Naming

Don't store two equivalently named recipes in different subfolders of your
repository. It would mean possibly building two different containers with the
same unique resource identifier, and then mixing them up.

## Container Building

The following are good practices for building containers.

**Practice container hygiene** 

Keeping Singularity Hub open and free for use, without limitations, is dependent on having an affordable cost structure. We pay for every file in storage, and every build, so it is not ideal to keep files or collections that won't be needed.

**Don't be root**

Generally don't install to the /root folder, as this might have issues when the 
container is executed in user space.

**Write commands in post**

It's best to put all install commands directly in the `%post` section, as opposed 
to within scripts that are executed from it. Depending on how you wrote your bash
script, it could be that an error in a script was not passed on to the build.

**Choose a building strategy that matches your development**

You should choose a build option (manual, automated on commit, or deployment)
based on your development strategy. If you have testing set up, deployment would
be logical, as containers will be built after your tests have passed. If you
commit to your repository regularly, you might consider a manual build to
only trigger when you have a version ready. 

**use tags instead of collections for image versions** 

Let's say that I have an image called `AnalysisGPU` and I've built it for two versions. Instead of creating two repostories on Github, `AnalysisGPU_version1` and `AnalysisGPU_version2, if I have two recipes, `Singularity.version1` and `Singularity.version2` under one repository called `AnalysisGPU`, then my images will be available as `AnalysisGPU:version1` and `AnalysisGPU:version2`
