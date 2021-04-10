---
title:  "Singularity Hub is going read-only"
date:   2021-04-10 4:40:21 -0500
categories: announcement
badges:
 - type: info
   tag: announce
---

As of early 2021, the creator and maintainer for 5 years of Singularity Hub [@vsoch](https://github.com/vsoch) left Stanford for a new opportunity, and she and the community have been working to figure out a future for the registry as she can no longer maintain it. Due to the shortage of time and because alternative builder services are available, Singularity Hub will soon be going "read-only" to support a migration to a read only registry.
For the purpose of reproducibility, namely ensuring that all containers that have been built and are currently in the registry will continue to exist at their respective `shub://` urls, we have a plan to do a migration, the timeline and details which are discussed in this post.

## Timeline

In a week, on Saturday April 17th 2021, Singularity Hub will stop reacting to the changes in 
your Singularity files, and will not build any new images.  The site will otherwise remain operational
for another week.

In two weeks, on Saturday April 24th 2021, Singularity Hub website will be turned down and will redirect
to an archival location hosting images that were existing before the freeze. By default, all images will be included, however if you would like to opt one or more of your collections out of being included, please [let us know](https://github.com/singularityhub/singularityhub.github.io/issues).

## Existing images

To support the efforts of reproducible science and ease migration to alternative solutions,
the current plan is to move already existing images to be served from the https://datasets.datalad.org/,
thanks to the infrastructure and bandwidth provided by the 
[Center for Open Neuroscience @ Psychology and Brain Sciences of Dartmouth College](http://centerforopenneuroscience.org/).

And no worries, [DataLad](http://datalad.org) will not be required, and `singularity pull shub://`
will be working as before, so no changes would be required to your scripts for already existing images. singularity-hub.org will continue to exist as a static, archival registry.

## Alternatives
While it could be possible to again provide builders for Singularity Hub, there are no plans to do this at this time. The following options are available if you want to continue building:
- Deploy your own [Singularity Registry Server (sregistry)](https://singularityhub.github.io/sregistry/)
- Build a Docker image and pull down to Singularity with the `docker://` unique resource identifier. You kill two birds with one stone.
- Build and upload Singularity images to GitHub releases: see https://github.com/singularityhub/singularity-deploy
- Use [datalad-container](https://github.com/datalad/datalad-container) to manage your built images, and host them across a wide range of storage solutions supported by DataLad or git-annex: see [the datalad handbook](https://handbook.datalad.org/en/latest/basics/101-138-sharethirdparty.html?highlight=gin%20rclone) for details. 
- A small free tier is available on the [Sylabs Cloud](https://cloud.sylabs.io/library)
