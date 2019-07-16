---
title:  "Singularity Hub 2.2 Release"
date:   2019-07-16 11:20:21 -0500
categories: release
badges:
 - type: info
   tag: release
 - type: warning
   tag: security
---


Singularity Hub v2.2 is a security release following a malicious use of the API. 
A quick summary of changes include:

 - limits have been set on all views, APIs, and user actions
 - login is required for most views
 - unnecessary views have been removed

A detailed description is included below, and for a listing of new limits we direct
the reader to the [limits]({{ site.baseurl }}/regulatory/limits) page.
See [best best practices]({{ site.baseurl }}/regulatory/best-practices) for instructions on how to properly pull containers so that you don't exceed a rate limit.

<!--more-->

## What Happened?

A single container pull from a private ip address using the Singularity Hub API 
incurred substantial charges that put the future of the service at risk.


## How did we respond?

The most conservate action was taken to protect the user base. The server was taken 
offline immediately, and all GitHub tokens revoked. The main application tokens 
for authentication were nullified. An investigation was immediately done by way
of information from the server, and in the following weeks there was another investigation
looking at bucket requests. We were unable to determine the final source of 
the activity.

## How do we move forward?

It is essential that we control all user requests moving forward. This means use
of the API, downloads of containers, and viewing of the website. The new
[limits]({{ site.baseurl }}/regulatory/limits) are listed briefly here:

### API Access

In three years, it wasn't an issue that any non authenticated user could pull a container.
However, now it is essential that this privilege is limited, meaning that storage
is entirely private, and only accessible via signed URLs from the server. Each 
container is allowed *100 GET requests* per week, globally. This means that it is
essential for you to interact with the API responsibly. Instead of issuing a run,
exec, or shell directly to a Singularity Hub unique resource identifier:

{% include alert.html type="danger" title="Don't do this!" content="<pre><code>singularity exec shub://collection/container
singularity run shub://collection/container
singularity shell shub://collection/container</code></pre>"%}

you should pull a container first, before running anything in batch or parallel,
and then interact with the binary directly:

{% include alert.html type="tip" title="This is good practice" content="<pre><code># First do this 
singularity pull shub://collection/container

# Then do this
singularity run container.sif
singularity shell container.sif</code></pre>"%}

If you interact with the API responsibly, you will not have any issues using
up quotas for the week. 

### Views Rate Limiting

In addition to API rate limiting, all views are rate limited, with the number
of allowed requests per ip address varying based on the view. You can see
specific limits [here]({{ site.baseurl }}/regulatory/limits). Once you reach
and then try to exceed the limit, you are blocked.

### Account Controls

To give users more control over their collections and webhooks, controls have been added to a user’s profile page to delete their account. Previously, they would need to delete container collections and revoke it from GitHub, so this does that for them. 

### API Endpoints

Previously, it was possible to get a listing of collections or containers (but without download addresses). We have removed these endpoints, along with extraneous endpoints
that don't directly contribute to serving containers.


### Collection Views

 - It used to be the case that collection pages were automatically refreshed to show containers building, or change in status. Given rate limiting on these pages, we no longer do this. You will need to refresh the pages on your own to see changes.
 - Download of containers from the interface is removed, and download of recipes is rate limited.
 - Views for container filesystem trees, and other size related tools are removed.
 - All views except for the home and about pages require login, and have rate limits.
 - Containers will no longer show file counts and extensions
 - Associated content (demos, videos, documentation, and asciinema) are disabled. Previously added demos will remain.


### Builders

A collection is already limited to have one active builder, however now they are also limited
to have only 5 in the pending state. As a user, you are allowed to have a maximum of 100 containers
per collection, and a total of 100 collections.

### Server Policy

In the case of normal usage, we expect monthly charges to be within a reasonable limit.
If charges for any given month exceed that limit, Singularity Hub will be temporarily shut
down until the start of the following month. Users will be noticied here, along with
on social networks (Twitter). 

## Does this include planned updates?

At the end of June, we sent out a user feedback survey to help determine next steps
for Singularity Hub. Although close in time, the changes described here are not related
to that survey. However, as we have started to look over the responses, a few 
of the points we are addressing with these changes, including:

 - a better documentation base
 - frequently asked questions

We are continuing to discuss these future plans and changes for Singularity
Hub, and will update the community as we progress!
