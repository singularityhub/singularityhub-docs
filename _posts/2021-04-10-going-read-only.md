echo ---
title:  "Singularity Hub is going read-only"
date:   2021-04-10 4:40:21 -0500
categories: announcement
badges:
 - type: info
   tag: announce
---

For the shortage of time and because alternatives are available, Singularity Hub will soon cease to exist in 
its current form.


## Time-line

In a week, on Saturday April 17th 2021, Singularity Hub will stop reacting to the changes in 
your Singularity files, and will not build any new images.  The site will otherwise remain operational
for another week.

In two weeks, on Saturday April 24th 2021, Singularity Hub website will be turned down and will redirect
to the location hosting already existing images.

## Existing images

To support the efforts of reproducible science and ease migration to alternative solutions,
the current plan is to move already existing images to be served from the https://datasets.datalad.org/,
thanks to the infrastructure and bandwidth provided by the 
[Center for Open Neuroscience @ Psychology and Brain Sciences of Dartmouth College](http://centerforopenneuroscience.org/).

And no worries, [DataLad](http://datalad.org) will not be required, and `singularity pull shub://`
will be working as before, so no changes would be required to your scripts for already existing images. 

## Alternatives

- Deploy your own [Singularity Registry Server (sregistry)](https://singularityhub.github.io/sregistry/)
- Build and upload Singularity images to GitHub releases: see https://github.com/singularityhub/singularity-deploy
- Use [datalad-container](https://github.com/datalad/datalad-container) to manage your built images, and host them
  across a wide range of storage solutions supported by DataLad or git-annex: see
  http://handbook.datalad.org/en/latest/basics/101-138-sharethirdparty.html?highlight=gin%20rclone 