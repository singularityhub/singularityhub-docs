---
title: "Recipes in Detail"
pdf: true
toc: false
---

# Recipes in Detail

Let's talk first about the starting unit of operation - writing a Singularity Recipe file.

## Writing a Recipe

Singularity Hub builds your [Singularity](https://www.sylabs.io/guides/3.2/user-guide/) containers from a build specification - a file named "Singularity" that is used to build a container. We call this recipe the Singularity Recipe. Singularity Hub makes this build process easy, without requiring transferring files or having superuser permissions on your part. You simply need to write a Singularity Recipe, put it in a Github repository, and then connect that repository to your account. Each repository maps to what Singularity calls a Container Collection, and in this collection you can maintain different tagged versions of your container build. Each time that you push, Singularity Hub will scan its contents for updated recipe files, and one or more new containers will be built by the [builders](../builds). Once the build is done, you can use the Singularity software to [pull your containers](https://www.sylabs.io/guides/3.2/user-guide/quick_start.html?highlight=pull#download-pre-built-images) from any command line. Let's take a look at the general workflow for doing this. Here we have a small repository with a Singularity Recipe named `Singularity`:


```bash
Bootstrap:docker  
From:ubuntu:16.04

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

## Multiple Recipes

The most basic thing you can do is have one recipe in your repository, the file `Singularity` mentioned above that will build one container on your master branch. Without providing an extension, this assumes a tag of `latest`, and so you given a username `vsoch` and repository `hello-world`, you would be building the image `shub://vsoch/hello-world:latest`. It also means that, if you are to create a *second* Singularity file anywhere in the repository:

```
hello-world/
         Singularity        (--- shub://vsoch/hello-world:latest
         subfolder/
              Singularity   (--- shub://vsoch/hello-world:latest
```

these two files are associated with the same unique resource identifier, and Singularity Hub will build the most recent. The file that was used is captured in the build metadata. The folder organization doesn't matter. However, if we create a second file with an extension to indicate a tag:

```
hello-world/
         Singularity           (--- shub://vsoch/hello-world:latest
         subfolder/
              Singularity.tag  (--- shub://vsoch/hello-world:tag
```

Then two images will be built, `shub://vsoch/hello-world:latest` and `shub://vsoch/hello-world:tag`. Thus:

{% include alert.html title="Tip" type="tip" content="It is recommended practice to have uniquely named recipe files, each starting with 'Singularity' with an extension to indicate a tag." %}

<br>

# Building your Container

After logging in to Singularity Hub with your GitHub account, you can [create a new collection](https://singularity-hub.org/collections/new), which means making a connection to some GitHub repository where you've put your files and data. This collection of builds, a Container Collection, is associated with a single repository. 

Let's say that I create a recipe in this repository. I would add the file, and push to GitHub.

```bash
git add Singularity  
git commit -a -m "Adding Singularity file."  
git push origin master  
```

Since I am in Automated Build mode(by default), the push will triger builds in Singularity Hub. 

Please note that in the Automated Build mode, Singularity Hub will only consider file changes from the push after the connection being established. That is, if one Singularity file is created before the connection, it would not be considered by Singularity Hub, unless you modified it in the following push.

Next, you might want to explore your [options for building](../builds) or read about [build limits](../regulatory/limits)
