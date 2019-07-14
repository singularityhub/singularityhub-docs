> Hi there, I'm one of the Singularity Hub robots! I'm here to help build your containers.


[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/robot_package.png]]
___

## Build Options
With version 1.0, you had one option - to build on each commit or disable entirely. This new version provides several options:

[Automated on Commit](Automated-Build): is the default and previously used option. All commits are assessed for new recipes, and then a new container is built for the commit. Previous containers with the same unique resource identifier (e.g. vsoch/hello-world:pasta) that are not frozen are re-created. New containers are created given a found frozen container. Conflicts with recipe names in the same repository are resolved by the date of the most recent file. You have the ability to re-trigger any build given that a build parameter has been changed (e.g., debug mode on).

[Automated on Deployment](Deployment-Build): If you use continuous integration to test and then issue a deployment commit, Singularity Hub (when using this option) will only build containers that have the deployment status. All other commits are ignored.

[Manual Trigger](Manual-Build): If you preference is to connect the repository and manually trigger builds, this is now possible. To deploy manually, the automatic building is disabled.

## What is a builder?
A builder is an instance, deployed on demand when it's needed, on Google Cloud.

 - **Machine Type**: The machine type is a [n1-standard-1](https://cloud.google.com/compute/docs/machine-types#standard_machine_types). This is a rather little guy, with 1 virtual CPU and 3.75 GB of memory. It works for building.
 - **Size** For the first version of Singularity Hub, given that builds needed to have size estimated, we also offered a larger 100GB instance. Now that we are using a more optimized compression format for image files, we are starting out trying just offering 50GB instances.
 - **Singularity Version** The new Singularity Hub supports building version 2.4 and up images, which are secure and read only. Legacy images ported from the old version (ext3) will be frozen, and the only different format.
 - **Operating System** The base OS is Ubuntu 16.04 with LTS.

The instance, for Singularity 2.0, comes with software and libraries essential for Singularity and the Singularity python software. We also install several modules (e.g., yum, deboostrap) for the build environment to address all the bugs that come with building certain images :)

Note that the build is done in an enclosed environment on the host, so you will not have any access to the host. This isolated environment will be available with future releases, and for those interested, for now you can look at it on it's [branch](https://github.com/cclerget/singularity/tree/feature-squashbuild-secbuild) maintained by a talented developer on our team. For the software in the environment, see it's [Singularity Recipe](https://github.com/cclerget/singularity/blob/feature-squashbuild-secbuild/secbuildimg.sh.in#L49).

To specify your builder and other settings discussed, you should edit a collection's [Build Settings](Build-Settings). If you have different needs than our default builders, please [reach out](https://github.com/singularityhub/singularityhub.github.io/issues).
