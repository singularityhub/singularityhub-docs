# Collections
A collection is a group of image that is built from Singularity Recipes discovered by the robots in a Github repository. One collection correspond to one repository, and may have multiple versions and tags.

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/collections/collections.png]]

Let's talk first about the starting unit of operating - writing a Singularity Recipe file.

# The Singularity Recipe

Singularity Hub builds your [Singularity](https://www.sylabs.io/guides/3.2/user-guide/) containers from a build specification - a file named "Singularity" that is used to build a container. We call this recipe the Singularity Recipe. Singularity Hub or your personal Singularity Registry server makes this build process easy, without requiring transferring files or having superuser permissions on your part. You simply need to write a Singularity Recipe, put it in a Github repository, and then connect that repository to your account. Each repository maps to what Singularity calls a Container Collection, and in this collection you can maintain different tagged versions of your container build. Each time that you push, Singularity Hub will scan its contents for updated recipe files, and one or more new containers will be built by (on Singularity Hub, the [builders](Meet-Builders)) or for Singularity Registry Server, Google Cloud Build. Once the build is done, you can use the Singularity software to [pull your containers](https://www.sylabs.io/guides/3.2/user-guide/quick_start.html?highlight=pull#download-pre-built-images) from any command line. Let's take a look at the general workflow for doing this. Here we have a small repository with a Singularity Recipe:

`Singularity`
```bash
Bootstrap:docker  
From:ubuntu:latest  

%labels
MAINTAINER Vanessasaur
SPECIES Dinosaur

%environment
RAWR_BASE=/code
export RAWR_BASE

%runscript
echo "This gets run when you run the image!" 
exec /bin/bash /code/rawr.sh "$@"  

%post  
echo "This section happens once after bootstrap to build the image."  
mkdir -p /code  
apt-get install vim  
echo "RoooAAAAR" >> /code/rawr.sh
chmod u+x /code/rawr.sh  
```

## Naming Recipes
**How should I name my Singularity Recipe Files?**
You should name them with an extension that indicates a tag of your choice, or none to default to latest. The extension of the Singularity file is your tag for your container, meaning that a single repository can serve multiple container builds! For example, for username `vsoch` and Github repository `hello-world`:


```
[user]/[repository]
vsoch/hello-world/                  <--- collection vsoch/hello-world

             .git/   
             science/                  
                  subfolder/             
                        Singularity       <--- vsoch/hello-world:latest
                        Singularity.gpu   <--- vsoch/hello-world:gpu

             fruits/                     
                  tomatoes/
                        Singularity.avocado       <--- vsoch/hello-world:avocado
                        Singularity.tomato        <--- vsoch/hello-world:tomato
```

Notice how the folder hierarchy doesn't matter. Recipes are found recursively, and we look for all Singularity files to build. The tags come by way of the extension of the Singularity files. If a user has a repeat tag, then the file with more recent changes (timestamp) is built. Only Singularity files that have been changed since the last commit will be built with [Automatic Building](Automatic-Build) in this fashion. If you want to go back int time to a particular commit, you should use a [Manual Build](Manual-Build). This means that if your push to Github has 10 changed Singularity recipe files, all 10 will be added to the queue to build, and your collection is allowed one active build at a time.

## A More Detailed Example
The most basic thing you can do is have one recipe in your repository, the file `Singularity` mentioned above that will build one container on your master branch. Without providing an extension, this assumes a tag of `latest`, and so you given a username `vsoch` and repository `hello-world`, you would be building the image `shub://vsoch/hello-world:latest`. It also means that, if you are to create a *second* Singularity file anywhere in the repository:

```
hello-world/
         Singularity        <--- shub://vsoch/hello-world:latest
         subfolder/
              Singularity  <--- shub://vsoch/hello-world:latest
```
these two files are associated with the same unique resource identifier, and Singularity Hub will build the most recent. The file that was used is captured in the build metadata. The folder organization doesn't matter. However, if we create a second file with an extension to indicate a tag:

```
hello-world/
         Singularity           <--- shub://vsoch/hello-world:latest
         subfolder/
              Singularity.tag  <--- shub://vsoch/hello-world:tag
```

Then two images will be built, `shub://vsoch/hello-world:latest` and `shub://vsoch/hello-world:tag`. Thus:

> it's recommended practice to have uniquely named recipe files, each starting with "Singularity" with an extension to indicate a tag.

# Building your Container

I would add this file to one of my Github repositories, and push to Github.

```
git add Singularity  
git commit -m "Adding Singularity file."  
git push origin master  
```

Then I would log in to Singularity Hub with my Github account (or a Singularity Registry Server with the `google_build` plugin enabled, and [add the repository](https://singularity-hub.org/collections/new). A collection of builds, a Container Collection, is associated with a single repository.

**Important** For Singularity Hub, your builder has a two hour build limit! After 2 hours, the build will be killed. For Google Cloud build, it depends on how the registry owner configured it.

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/container-build.png]]

If my repository is already connected, then the push is sufficient to trigger the build. I don't need to visit the interface.

**What are my options for building?**
Singularity 2.4 introduces multiple options for building, in addition to the automatic (by commit) default:

 - [Automated Build](Automated-Build): build containers automatically from Github commits
 - [Manual Build](Manual-Build): Only build when you issue the command.
 - [Deployment Build](Deployment-Build): Triggered only by a deployment from Github.

Each has different use cases, and you should read about each of the above to know what is best for you. For most users, the Automated Build is preferred, but if you want to do testing on your own first, you might choose Deployment Build. If you want to explicitly select commits and recipes and when to do it, or to go back in time to build a previous container, you will want to enable manual build. You are free to change your building selection at any time.

**What are my limitations for building?**

For Singularity Hub, We have various security checks to ensure that you are not acting in a way that is deemed malicious. If you do, your collections will be disabled from building. For each collection, we also allow one active build at a time, and a limit of 5 builds you can have in the active and pending queue at a time. If you ever are concerned about a time out or want to try "pinging" your queue, you can click the ["refresh" icon](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Refresh_font_awesome.svg/512px-Refresh_font_awesome.svg.png) in the container controls. Singularity Hub is a build service provided without charge, and so we strongly recommend that you use it responsibly. Please see our [terms](https://www.singularity-hub.org/terms) for usage.

**Where can I learn about the builders?**
We have a section where you can [meet the builders](Meet-Builders). Generally, you can customize the instance size, and if you want debug output. Since squashfs are flexible and we don't need a size in advance, you shouldn't need to specify an image size.

### Add Tests
Singularity Hub will do a basic test to see if your container runs a command to list files. This test will return that the build is successful given a return code 0, and this is the approach that we must take to support a wide variety of containers with different OS inside. However, you can imagine cases where a container might have an otherwise working OS, but perhaps run out of room extracting some data and erroneously report as correct. For this reason, we encourage you to write your own [tests for your container](http://singularity.lbl.gov/docs-recipes#test) to be sure that everything works as you would expect. If these tests don't pass, the build will fail as you would want. On the other hand, if you want the build to appear successful so you can pull and inspect the container, you can remove these tests.


## Referencing Containers
Singularity Hub maintains a uri scheme similar to Docker and Github to give it's containers, within the scope of a single registry, a unique uri. The convention is the following:

```
shub://<registry>/<username>/<reponame>:<tag>@digest
```
 - `username`: maps to your Github username, or in the case of an organization repository, the organization. We use Github's namespace so we don't need to maintain our own, or manage permissions for editing the recipes.
 - `reponame`: is the name of the repository.
 - `tag`: corresponds to the extension of the Singularity file. In the old Singularity hub, it corresponded to a branch.
 - `digest`: refers to a commit id or a container (md5 sum) hash.
 - `registry`: is the host URL where the container is being served. 

For registry, if you don't have https, then when you pull the image with singularity you would need to specify to not use it:

```
SINGULARITY_NOHTTPS=yes singularity pull shub://vsoch/hello-world
```

A full example unique resource identifier would look like:

```
shub://singularity-hub.org/vsoch/hello-world:latest@<commit>
```

but since Singularity Hub (singularity-hub.org) is the default for `<registry>`, you typically don't need to specify it:

```bash
shub://<username>/<reponame>:<tag>@digest
shub://vsoch/hello-world:latest@<commit>
```

If your Singularity Registry Server is different, just say so!

```bash
singularity pull shub://containers-ftw.org/vsoch/hello-world
```

Regardless of how you build, each is available programatically from the command line, and for interactive viewing from the browser:

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/container-builds.png]]

## Managing Images
You likely want control to tag and "freeze" an image to ensure that it is available and not removed. You can do this via the Singularity Hub collection view.

### Ephemeral Images
**How do I maintain versions of my images?**
You must freeze all images that you wish to keep an exact version for.

New for Singularity 2.0 are ephemeral images. If you push an image with tag `gpu` and then push another commit, the previous image will be replaced. This change addresses the observation for early Singularity Hub that many users would not clean up redundant builds. Never fear, you can still keep as many as you like, but you need to specify them! In the web interface if you select any container build to "freeze"

[imagehere freeze]

Any subsequent pushes with a Singularity Recipe that would have over-ridden an image will in fact produce a new image file, the same uri but referenced by a different commit:

```
shub://vsoch/hello-world:gpu@123  <-- frozen
shub://vsoch/hello-world:gpu@234
```

If gpu commit `123` was not frozen, it would completely be replaced by commit `234`. You are free to freeze as many images as you like. Frozen images cannot be edited or changed, as it is an assurance of being just that. Frozen.

### Customizing and Updating your Builder
There are a few important variables that you can change the default settings for. When we update the builders on Singularity Hub (for example, when a new version is released) all new collections will use the latest builder, by default. However, collections that are already existing will not be updated automatically, as this may not be your preference. To edit your builder, you can do so by clicking on the build [Settings](Build-Settings) button in your collection management bar.

Here you can change many options:
*   **Builder**: By default, we use an Ubuntu 16.04 image with debootstrap and yum installed, and Singularity 2.3. You can change that in this view. We currently provide the builders, and if you find none are suitable, please [submit an issue](https://github.com/singularityhub/singularityhub.github.io/issues/new) and we will figure out a solution right away! 
*   **Debug**: Set debug to true to make the output of the builder more verbose.


### Best Practices

We are providing this builder as a service, and would greatly appreciate the following:

*   **Practice container hygiene**. Keeping Singularity Hub open and free for use, without limitations, is dependent on having an affordable cost structure. We pay for every file in storage, and every build, so it is not ideal to keep files or collections that won't be needed.
*   **use tags instead of collections for image versions** Let's say that I have an image called `AnalysisGPU` and I've built it for two versions. Instead of creating two repostories on Github, `AnalysisGPU_version1` and `AnalysisGPU_version2, if I have two recipes, `Singularity.version1` and `Singularity.version2` under one repository called `AnalysisGPU`, then my images will be available as `AnalysisGPU:version1` and `AnalysisGPU:version2`