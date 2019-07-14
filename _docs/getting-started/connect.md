---
title: Connect with GitHub
pdf: true
toc: false
---

# Repository Connection

## GitHub Repository

If you remember from the [terms]({{ site.baseurl }}/about#terms) page, a collection
is a a group of image that is built from Singularity Recipes discovered by the robots in a Github repository. One collection correspond to one repository, and may have multiple versions and tags.

![Collection](https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/collections/collections.png)

You can create collections by logging in with GitHub to Singularity Hub.
You are free to choose allowing Singularity Hub to build public or private
collections. 


## Tags and Branches


By default, Singularity Hub will build from the master branch. You can choose
to enable more branches in the collection settings.

```bash
Collection --> Settings --> Edit Branches
```

You can then turn on (and off) branches to build. This is also helpful if you want to disable a branch for building, while testing, or something like that:

![Collection](https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/collection-branches-details.png)

Branches are a useful way for allowing automated building without having building happen for every single commit. If you have your production branch enabled for building, it will only build final images when you commit to it.


# Recipes

Before we walk through a [recipe example](recipes), let's talk about how
you should name and organize recipe files in your repository.

## Naming Recipes

**How should I name my Singularity Recipe Files?**

The *extension* of the Singularity file will correspond with the container tag.
This is an easy way to represent the full information about the container in
the GitHub repository. If you provide a `Singularity` recipe without an extension,
this builds to the tag "latest." This means that a single repository can serve 
multiple container builds! For example, for username `vsoch` and Github repository `hello-world`:

```bash
[user]/[repository]
vsoch/hello-world/                        (--- collection vsoch/hello-world

             .git/   
             science/                  
                  subfolder/             
                        Singularity       (--- vsoch/hello-world:latest
                        Singularity.gpu   (--- vsoch/hello-world:gpu

             fruits/                     
                  tomatoes/
                        Singularity.avocado       (--- vsoch/hello-world:avocado
                        Singularity.tomato        (--- vsoch/hello-world:tomato
```

Notice how the folder hierarchy doesn't matter. Recipes are found recursively, 
and we look for all Singularity files to build. 

**What if I have two recipes with the same name?**

You should avoid doing this, because Singularity hub will select to build the 
file with more recent changes. If you use {% include doc.html name="automated builds" path="build/automated" %},
this is the method to select recipes to build. If you want to go back in time
to a particular file and commit, you should use a {% include doc.html name="manual build" path="build/manual" %}
You can read more about builds {% include doc.html name="here" path="build" %}.

**How many containers can I build at once?**

Each container collection maintains its own queue, so you are allowed one build
at a time per collection. This means that if you include 5 recipes in a repository,
each requiring a build, all ten will be added to the queue and processed in serial.

You should now read a [more detailed example](recipes) for creating your recipe,
[naming containers](naming), [managing images](manage) or read about your options for [building](../builds).
