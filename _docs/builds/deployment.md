---
title: "Builds: Deployment"
pdf: true
toc: false
---

# Deployment Builds

If you use continuous integration to test and then issue a deployment commit, Singularity Hub (when using the "deployment" trigger) will only build containers that have the deployment status. All other commits are ignored.

## Overview

Any continuous integration service that implements interacts with [GitHub deployment
API](https://developer.github.com/v3/repos/deployments/) may be able to create deployments
for you. Usually you must enable this through their configuration.

Examples of providers:

 * [Travis-CI](https://docs.travis-ci.com/user/deployment/releases/)
 * [Jenkins](https://resources.github.com/whitepapers/practical-guide-to-CI-with-Jenkins-and-GitHub/))

For providers that don't have internal support, there are [tools](https://circleci.com/blog/publishing-to-github-releases-via-circleci/) that you can use to still interact with the API.

## Internals

The only difference between a deployment and commit trigger is that the deployment trigger checks if the webhook came via a deployment commit, which only happens under the condition of services using the deployment API. This kind of build is best suited for those that want to deploy fully tested and released versions of their containers. Another (similar) approach would be to deploy on push to master (the standard commit trigger) and then only allow push to master to happen after continuous integration and testing have completed. Both of these approaches are strongly encouraged.

## Deployment through Docker Hub
Another good example use case is deploying a container to Docker Hub first. The flow of events might look like the following:

```
[1. Github commit] --> [2. Successful build Docker Hub] --> [3. Github receives status] --> [4. Deployment Webhook to Singularity Hub]
```
 - For step 1, we might make changes to a Dockerfile that describes our image, and perhaps the Singularity file in the repository simply builds using that as a base.
 - For step 2, Docker Hub completes the build of the Docker image
 - And in 3. sends a success status back to Github.
 - Github then sends a Deployment webhook to Singularity Hub

Note that Github would still send the push event (step 1.) to Singularity Hub, but finding that the build trigger is "deployment" it is ignored. Also note that you could easily
pull your container down to Singularity directly from Docker Hub, and kill two birds
with one stone.

## Manually creating a deployment

If you have a custom continuous integration set up, or are using a provider you
may be able to create a deployment event yourself using the [GitHub repos REST
API](https://developer.github.com/v3/repos/deployments/#create-a-deployment) or
one of the [tools](https://circleci.com/blog/publishing-to-github-releases-via-circleci/) 
mentioned previously.

## Troubleshooting

#### Deployment build not being triggered

This could happen because of a problem with communication from the continuous integration service to GitHub or from Github to Singularity Hub (see *Internals*).

#### No message from continuous integration service to GitHub

Check you are using a supported continuous integration service and it is
configured correctly, or set up manual deployments using the REST API.

#### No message from GitHub to Singularity Hub

Verify in your repository webhook settings that you have a Singularity Hub webhook that is configured to send Deployment events. This is enabled by default for newly-linked repositories, but it may be the case that you need to set it yourself if you linked the repository to Singularity Hub in the past. There was also a change in webhook format to remove the "www" prefix - see our [webhooks](https://singularityhub.github.io/singularityhub-docs/docs/support/troubleshooting#why-isnt-my-webhook-working) section in the troubleshooting
guide for more detail.

You can see a log of sent webhooks and the responses from SingularityHub on GitHub in Settings > Webhooks > Edit (`https://singularity-hub.org/...`).

More information on webhook events is available in GitHub's [webhook documentation](https://developer.github.com/webhooks/#events).
