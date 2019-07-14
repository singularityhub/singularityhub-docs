---
title: "Builds: Deployment"
pdf: true
toc: false
---

# Deployment Builds

If you use continuous integration to test and then issue a deployment commit, Singularity Hub (when using this option) will only build containers that have the deployment status. All other commits are ignored.

## Overview

A deployment build works similarity to a commit (automatic) build and indeed is also automatic, however it checks if the webhook came via a deployment commit first, and the build only happens under this condition. This kind of build is best suited for those that want more control (e.g., continuous integration and testing) before serving their finished containers, and strongly encouraged. Another good example use case is deploying a container to Docker Hub first. The flow of events might look like the following:

```
[1. Github commit] --> [2. Successful build Docker Hub] --> [3. Github receives status] --> [4. Deployment Webhook to Singularity Hub]
```
 - For step 1, we might make changes to a Dockerfile that describes our image, and perhaps the Singularity file in the repository simply builds using that as a base.
 - For step 2, Docker Hub completes the build of the Docker image
 - And in 3. sends a success status back to Github.
 - Github then sends a Deployment webhook to Singularity Hub

Note that Github would still send the push event (step 1.) to Singularity Hub, but finding that the build trigger is "deployment" it is ignored.

## Configuring your GitHub repository

Deployment builds occur when a deployment event is raised by GitHub and received by Singularity Hub. In order to cause a deployment event to be raised, you may follow the steps outlined [here](https://developer.github.com/v3/guides/automating-deployments-to-integrators/#sending-deployments-whenever-you-push-to-a-repository) to setup GitHub's Auto-Deployment integration for your repository. 

This will require a personal access token, which can be created by following the steps [here](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

The GitHub Auto-Deployment integration can be configured to only send the deployment event after receiving a success status from your CI server.

**Note**: this integration only triggers a deployment event when pushes are made to your repository's default branch (usually master).

## Troubleshooting

### Deployment build not being triggered

Verify in your repository webhook settings that you have a Singularity Hub webhook that is configured to send Deployment events. This is enabled by default for newly-linked repositories, but it may be the case that you need to set it yourself if you linked the repository to Singularity Hub in the past.

More information on webhook events is available in GitHub's [webhook documentation](https://developer.github.com/webhooks/#events).
