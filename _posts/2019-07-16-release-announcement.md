---
title:  "Singularity Hub Announcement"
date:   2019-07-16 10:25:21 -0500
categories: announcement
badges:
 - type: info
   tag: announce
---

I'm am happy to announce that we will cautiously be opening Singularity Hub back up today after quite
an eventful July. This version of Singularity Hub follows a malicious use 
of our API that indeed did put the future of the service at risk. I say cautiously
because we have disabled building for all except for a group of testing users,
and will plan on re-enabling building for everyone later this week, or sometime next week.
If you are interested in being a testing user, please let me know. 
Through the end of this week I'm going to be monitoring the service 
like a hawk, and all users should expect intermittent restarts if necessary.

## Quick Links

For quick links, see the new [limits]({{ site.baseurl }}/docs/regulatory/limits)
and [release notes]({{ site.baseurl }}/2019/security-release/).

<!--more-->

## Your Expectations

I want to remind everyone that this is a build service primarily for academics 
and small groups to create and then use their containers. It is developed
at Stanford University and not an enterprise service.
With this in mind, I appreciate everyone's patience as I've been doing this work. 
It really has been a night and day sort of deal, and I've been doing
absolutely everything in my power to move forward from the event.
There have been quite a lot of changes to the server, so if you run into an issue
please [let me know](https://www.github.com/singularityhub/singularityhub.github.io/issues)
so I can address it promptly. 

## Changes

Moving forward we have to ensure the longevity of the resource. Thus 
I have implemented limits and quotas with respect to usage of
the server, and it's essential that you read about these in our new [limits]({{ site.baseurl }}/docs/regulatory/limits) page on the documentation site. Briefly,
there are no longer public containers in storage, and access is controlled
via the main server. Most views require a login, and are rate limited.
Extraneous tools, views, and functionality has been removed. Before you
interact with an shub unique resource identifier, you need to think.
Any given container has a weekly allowance of 100
GET requests, and that's it. This means that instead of issuing an exec
or a run to a shub unique resource identifier, you need to pull it first,
and then interact with the container binary.  Moving forward it's
essential that you understand these limits, and follow best practices for
requesting to use containers so you essentially don't use up any quota.

You'll also notice this beautiful, new documentation site! 
In response to user feedback, I've created these better organized and 
searchable documentation pages.

I'm hopeful moving forward that users will interact with our service and APIs
responsibly. I gave out a survey for feedback at the end of June, and although
the timing is ironic, that is in fact a different thing. In the next few
weeks, months, as we figure out updates for Singularity Hub in response
to this feedback, I will keep everyone informed! In the meantime,
if you have a question, or would like to be added as a tester, or just
want to say hello, please don't hesitate to reach out.


### Resources

 - [Limits]({{ site.baseurl }}/docs/regulatory/limits)
 - [Release Notes]({{ site.baseurl }}/2019/security-release/)
 - [Open an Issue](https://www.github.com/singularityhub/singularityhub.github.io/issues)
