Welcome to the Singularity Hub Wiki!

## Frequently Asked Questions

### What is Singularity Hub?
Singularity Hub is a registry for scientific [linux containers](https://opensource.com/resources/what-are-linux-containers).

### What is a Linux Container?
A container image is an encapsulated, portable environment that is created to distribute a scientific analysis or a general function. Containers help with reproducibility of such content as they nicely package software and data dependencies, along with libraries that are needed. Thus, the core of Singularity Hub are these Singularity container images, and by way of being on Singularity Hub they can be easily built, updated, referenced with a url for a publication, and shared. This small guide will help you to get started building your containers using Singularity Hub and your Github repositories.

### How is Singularity Hub related to Sylabs and Singularity?

[Sylabs](https://sylabs.io) is the company that maintains the open source [Singularity](https://www.github.com/sylabs/singularity) code base, along with providing services for paying customers that use Singularity containers. The creator and maintainer of Singularity Hub was one of the original Singularity (open source software) developers, however she is not affiliated with the company Sylabs. Singularity Hub continues to be maintained by this individual at Stanford University, with generous support from Google Cloud. The two now co-exist peacefully, both passionate about using and supporting users to build Singularity containers. The distinction between Sylabs Library and Singularity Hub (and [Singularity Registry Server](https://www.github.com/singularityhub/sregistry)) comes down to the intended communities that are served. Singularity Hub and Registry is a non-enterprise solution that is catered for research scientists.

### Is Singularity Hub open source

The source code for Singularity Hub is private, however the author has released an open source (somewhat) equivalent in the case that you want to deploy your own registry, [Singularity Registry Server](https://www.github.com/singularityhub/sregistry).

## Getting Started

### How does it work?

You can follow some basic steps to build your scientific containers.

   1. create an account on Singularity Hub, authentication is with Github
   2. connect a Github repository to a new container collection. This means creating a webhook that will trigger a new build at each push to the repository. One collection coincides with one repository, and holds multiple images.
   3. push the Singularity build recipe (a text file specification to build your image called `Singularity`) to the repository to trigger the build
   - Singularity-Hub will be notified of the push via the webhook, and the build recipe file will be version controlled via the Github commit. This repo url, commit id, storage location, and some secrets are then sent to Singularity Hub builders.
   - the container is built on the cloud, and when finished, is accessible programatically from the Singularity command line client, or the [Singularity Global client](https://singularityhub.github.io/sregistry-cli).
   - By default, we only listen for changes to the master branch. To enable other branches, you need to add them in your Collection settings.

Singularity Hub is developed at Stanford University with support from Google Cloud. Thank you!


## News
Singularity 2.5 is released, and so is Singularity Hub 2.1! See the [Release Notes](Release-Notes) for details.
We are collecting feedback for a potential update to Singularity Hub. [Please provide it here](https://forms.gle/ABEtT17esnMjkBx6A).

___

## Getting Started
This is the official user guide for [singularity-hub.org](https://singularity-hub.org), providing more detail on how to build, manage, and deploy your images. For quick answers, see our [Troubleshooting](Troubleshooting) page.

 - [Generate](Build-A-Container): How to connect to Github and create automatic builds for your containers.
 - [Visualize](Visualize-your-Container): Use Singularity Hub and command line tools to see the guts of your container.
 - [Deploy](Deploy): Use the Singularity software to use your containers from Singularity Hub.
 - [Share](Share-Containers): Share your containers via social media, and upload asciicasts to distribute.


## Resources
- [singularity software](https://singularityware.github.io) is going to be the controller for pulling and otherwise interacting with your images on Singularity Hub.
- [singularity global client](https://singularityhub.github.io/sregistry-cli) is another option if you want a manager included (a small database to keep track of things).
- [singularity server](https://singularityhub.github.io/sregistry) is an open source version of Singularity Hub if you want to deploy your own (that you can push to).
- [Troubleshooting](Troubleshooting) in case you need some help!
- [Terms of Service](http://singularity-hub.org/terms)

___

## Meet the Builders
We deploy instances on Google Cloud, our "builders" on demand to build your image. Each builder performs the build in a secure environment, and can be used in different ways! You can [read more about the builders](Meet-Builders), or continue to learn about your building options.

 - [Build Settings](Build-Settings): is where you customize your builders and collection preferences.
 - [Automated Build](Automated-Build): build containers automatically from Github *commits*
 - [Manual Build](Manual-Build): Only build when you issue the command.
 - [Deployment Build](Deployment-Build): Triggered only by a deployment from Github.