---
title: Singularity Hub Documentation
---

# Troubleshooting

 - [How can I best ask for help?](#how-can-i-best-ask-for-help)
 - [Why is my container stuck in waiting?](#why-is-my-container-stuck-in-waiting)
 - [Why isn't my webhook working?](#why-isnt-my-webhook-working)
 - [Why don't all my commits build images?](#why-dont-all-my-commits-build-images)
 - [Why did my build fail?](#why-did-my-build-fail)
 - [Why did my build succeed but my container is broken?](#why-did-my-build-succeed-but-my-container-is-broken)

## How can I best ask for help?

If you are having trouble with a build, you can of course [open an issue](https://github.com/singularityhub/singularityhub.github.io/issues) and @vsoch and the robots will help to the best that they can. 
We ask that you test building your container locally:


```bash
$ sudo singularity build container.sif Singularity
```

And check any logs retrieved from the server first.


## Why is my container stuck in "waiting"

When you start and stop builds quickly, such as committing to create a container, and then immediately committing again, the instance sometimes is delayed in coming down, and as a result the (equivalently named) instance is not created. This is a bug that we will fix, and in the meantime if you run into this issue you can cancel a currently running build first and then commit. If you already triggered the error, simply delete the container and build again.

## Why isn't my webhook working?

When we updated Singularity Hub to include new [limits](https://singularityhub.github.io/singularityhub-docs/2019/release-announcement/), we enforced using a singularity-hub.org URL without the "www", meaning that
going to https://www.singularity-hub.org will redirect to https://singularity-hub.org. This means
that if you created a GitHub webhook before July 2019, it might have a "www" and will return a 301 response:

```
Connection: keep-alive
Content-Length: 185
Content-Type: text/html
Date: Mon, 23 Sep 2019 16:21:21 GMT
Location: https://singularity-hub.org/hooks/build/push
Server: nginx/1.13.5
Body
<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.13.5</center>
</body>
</html>
```

The fix is to go into your GitHub repository "Settings" and under Webhooks, select
the webhook for Singularity Hub and remove the "www." More details are available
with the [announcement](https://singularityhub.github.io/singularityhub-docs/2019/open-for-building/#webhooks)
from July 2019.

For collections that were migrated (and existed on the testing server before it was converted to `singularity-hub.org`) the webhooks need to be updated for the different server address. The first obvious thing to try is logging out, and back in again. You can also navigate to the Settings --> Applications --> OAuth applications page and look at the configuration and response for the hook to better understand any issues. If you have no legacy images and don't mind rebuilding, then just delete the collection, log out and back in, and authenticate again. If you have legacy images (and don't want to delete the collection) then you can either make a second collection to work from, or wait for @vsoch to carefully update the webhooks.

## Why don't all my commits build images?

If you are ever confused about why a build seems to be over-writing another, remember that the default behavior is to do this **unless you specify to freeze an image*. Also be careful to check your build trigger settings. For example, if you are set to build manually, builds will not happen when you commit.

## Why did my build fail?

Builds generally fail for the following reasons:

 - you've exceeded the two hour limit, at which point the builder is terminated
 - the secure build environment doesn't support an operation or base library that you need
 - you used up all of the disk space, and the instance was terminated

The Singularity Hub robots do their best to support the large majority of use cases, but
we are not able to support custom build environments. If you have a request for a change, it's suggested
that you get community support first.

## Why did my build succeed but my container is broken?

We have seen rare cases when a portion of a build fails, and due to the way that
the recipe is written, it doesn't trigger Singularity to exit with a failure. 
For these cases, we recommend that you write tests to properly test that your container
functions as intended, and that you carefully look over build logs.
