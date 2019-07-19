---
title:  "Singularity Hub Open for Building"
date:   2019-07-19 4:40:21 -0500
categories: announcement
badges:
 - type: info
   tag: announce
---

Singularity Hub is open for general building! We have a few other updates to announce for the server.

## GitHub Login

You now shouldn't log out when you change between www and a naked domain (e.g.,
singularity-hub.org vs. www.singularity-hub.org), and the server will redirect
to https://singularity-hub.org.

## Webhooks

Importantly, since we now use a naked URL, all previously existing webhooks for your repositories
will need to be checked and updated if necessary to not include www. You can go into your GitHub repository Settings --> Webhooks, and then select the Singularity Hub webhook and change the url to not use www.,
and click save. For example, if you see this:

```
https://www.singularity-hub.org/hooks/build/push
```

You need to change it to:

```
https://singularity-hub.org/hooks/build/push
```

If the url is already correct, no further changes are required. And if 
you would like for us to do this for you, please
log in with GitHub, and then contact @vsoch with your collection name.
If you look at a webhook status and the response is red with 301, 
this means that you need to make this change.

{% include alert.html type="Warning" content="Your webhook will not ping a build if you do not do this!"%}

## Pulling from Singularity Hub

Again, we'd like to remind users that each container has 100 pulls per week, and that's
it. Please pull a container binary _before_ running anything at scale, and then
for your jobs interact with the container binary directly. If you have a special need
for a larger number of requests (e.g., a workshop or tutorial) please contact @vsoch
with the date to arrange it. Finally, we direct the reader to the following major 
updates about general usage limits.

## Collection Build Limits

Each collection is allowed a maximum of 25 builds per day. This includes rebuilds
of the same container. The overall collection limit of 100 containers is still set.

## Mobile Navigation

I've done my best to fix the mobile navigation so it's easier to see the items
on the left side. Thanks to @yarikoptic for pointing this out.

### Resources

If you run into an issue, please report it! @vsoch will be actively around
over the weekend to help.

 - [Limits]({{ site.baseurl }}/docs/regulatory/limits)
 - [Release Notes]({{ site.baseurl }}/2019/security-release/)
 - [Open an Issue](https://www.github.com/singularityhub/singularityhub.github.io/issues)
