---
title: "Limits"
pdf: true
toc: false
---

# Limits

Singularity Hub is a build service provided without charge, and so we strongly recommend that you use it responsibly. Please see our [terms](usage-agreement) for usage. We set limits
on usage of the APIs and general views to ensure that the service cannot be manipulated
in any way that might compromise its long term sustainability.

## API Limits

### Get Limit

Every single download of a container is counted toward the weekly limit for that container,
and the collection, which is set at 50 for each. This means that if you run an exec/run/shell with a container unique
resource identifier instead of directly to a container binary, you will meet the quota
almost immediately and not be able to interact with it for a week.

As an example, a working pull will look like this:

```bash
$ singularity pull shub://singularityhu/
INFO:    Downloading shub image
 764.00 KiB / 764.00 KiB [=============================================================================] 100.00% 3.10 MiB/s 0s
```

A pull that exceeds the quota will return a permission denied response (an integer 426)
that the Singularity client doesn't know how to parse:

```bash
INFO:    Downloading shub image
FATAL:   While pulling shub image: Failed to get manifest from Shub: json: cannot unmarshal number into Go value of type cliee
```

Additionally, the GET request on the server will not allow you to issue more
than 10 requests per minute. We understand that this limits your usage, but
with a proper setup to pull before you use your container, you really shouldn't need
anywhere near that number of pulls. If you have a special request to pull or otherwise
interact with a particular container (e.g., you are giving a workshop with 300
participants that need to pull in a single day) please 
contact @vsoch directly to ask for a special allowance.

## Building Limits

### Collection Build Queue

For each collection, we allow one active build at a time, and a limit of **5** builds you can have in the active and pending queue at a time. If you ever are concerned about a time out or want to try "pinging" your queue, you can click the ["refresh" icon](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Refresh_font_awesome.svg/512px-Refresh_font_awesome.svg.png) in the container controls. 

### Collection Daily Build

Each collection is allowed 10 total builds per day. You can see your daily count
in your Collection settings.

### Time Limit

Your builder has a two hour build limit. After 2 hours, the build will be killed.
If you use up disk space on your instance to deem in unable to commit to this time
period, it will be automatically killed by Google at a 3 hour mark, with no log returned.

### Container and Collection Limits
You are allowed a maximum of 100 containers per collection, and 100 collections.

## Views Limits

All views are rate limited, with the quantity depending on the view, Specific values
are included below:

 - **Apps, Tags, and Labels**: 10/day
 - **Home Page**: 100/day, 5/minute
 - **Demos** (deprecated): 10/day
 - **Single Collection view** 100/day
 - **Other Collection views** (e.g., settings, change status): 25/day
 - **Container views** 50/day
 - **User profiles** 10/day
 - **Trigger**: 10/day

After these limits are exceeded, you will be blocked.

## Global Limits

If Singularity Hub, at any point during the month, exceeds its monthly allowance,
all services will be shut off until the start of the next month. Given
responsible usage, this should not be an issue.
