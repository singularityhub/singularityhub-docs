---
title: "Builds: Deployment"
pdf: true
toc: false
---

# Deployment Builds

If you use continuous integration to test and then issue a deployment commit, Singularity Hub (when using this option) will only build containers that have the deployment status. All other commits are ignored.

## Overview

Any continuous integration service which implements the [GitHub checks
API](https://developer.github.com/v3/checks/) may be able to create deployments
for you. Usually you must enable this through their configuration.

Providers known to support the GitHub checks API:

 * Travis-CI ([Docs](https://docs.travis-ci.com/user/deployment/releases/))

Providers known to not yet support the GitHub checks API:

 * Gitlab-CI (See [issue #22187](https://gitlab.com/gitlab-org/gitlab/-/issues/22187))

## Internals

A deployment build works similarity to a commit (automatic) build and indeed is also automatic, however it checks if the webhook came via a deployment commit first, and the build only happens under this condition. This kind of build is best suited for those that want more control (e.g., continuous integration and testing) before serving their finished containers, and strongly encouraged. Another good example use case is deploying a container to Docker Hub first. The flow of events might look like the following:

```
[1. Github commit] --> [2. Successful build Docker Hub] --> [3. Github receives status] --> [4. Deployment Webhook to Singularity Hub]
```
 - For step 1, we might make changes to a Dockerfile that describes our image, and perhaps the Singularity file in the repository simply builds using that as a base.
 - For step 2, Docker Hub completes the build of the Docker image
 - And in 3. sends a success status back to Github.
 - Github then sends a Deployment webhook to Singularity Hub

Note that Github would still send the push event (step 1.) to Singularity Hub, but finding that the build trigger is "deployment" it is ignored.

## Manually creating a deployment (unsupported)

If you have a custom continuous integration set up, or are using a provider you
may be able to create a deployment event yourself using the [GitHub repos REST
API](https://developer.github.com/v3/repos/deployments/#create-a-deployment).

## Troubleshooting

### Deployment build not being triggered

This could happen because of a problem with communication from the continuous integration service to GitHub or from Github to Singularity Hub (see *Internals*).

#### No message from continuous integration service to GitHub

Check you are using a supported continuous integration service and it is
configured correctly, or set up manual deployments using the REST API.

#### No message from GitHub to Singularity Hub

Verify in your repository webhook settings that you have a Singularity Hub webhook that is configured to send Deployment events. This is enabled by default for newly-linked repositories, but it may be the case that you need to set it yourself if you linked the repository to Singularity Hub in the past.

You can see a log of sent webhooks and the responses from SingularityHub on GitHub in Settings > Webhooks > Edit (`https://singularity-hub.org/...`).

More information on webhook events is available in GitHub's [webhook documentation](https://developer.github.com/webhooks/#events).
