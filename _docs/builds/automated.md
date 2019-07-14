---
title: "Builds: Automated"
pdf: true
toc: false
---

# Automated Builds

Automated build, or building on commit, is the default. All commits are assessed for new recipes, and then a new container is built for the commit. Previous containers with the same unique resource identifier (e.g. vsoch/hello-world:pasta) that are not frozen are re-created. New containers are created given a found frozen container. Conflicts with recipe names in the same repository are resolved by the date of the most recent file. You have the ability to re-trigger any build given that a build parameter has been changed (e.g., debug mode on).

Singularity Hub has a default builder option to build automatically. This means that after you connect a repository, each time that you commit Singularity Hub will look for new recipes. The logic is the following:

## Finding Recipes

 1. we filter down to new commits since the last push
 2. of the previous commits, if any of them have files added or updated that match the pattern `Singularity` or `Singularity.<tag>`, we consider this a change to a Singularity Recipe that warrants rebuild.

When we finish the above steps, we have a basic dictionary of contenders that we've found. For example. here we have a `Singularity` Recipe at the base of the repository, and a second of the same name in a subfolder.

```python
     {'Singularity': {
                      'commit': 'bdcdb2ba436d43229dc5ee296d85dedc5aee6f5a',
                      'date': '2017-09-10T02:51:16Z',
                      'name': 'singularityhub/hello-world',
                      'url': 'https://github.com/singularityhub/hello-world/raw/bdcdb2ba436d43229dc5ee296d85dedc5aee6f5a/Singularity'
                      },

     'subfolder/Singularity': {
                      'commit': '3f4d63cadba1b3a6867868f9a85417751baba521',
                      'date': '2017-07-08T02:43:06Z',
                      'name': 'singularityhub/hello-world',
                       'url': 'https://github.com/singularityhub/hello-world/raw/3f4d63cadba1b3a6867868f9a85417751baba521/subfolder/Singularity'
                       }
    }
```

If you are thinking "Isn't there a conflict?" you are correct. The recipe files are named equivalently, and would both produce a container `shub://vsoch/hello-world:latest`. Thus, to resolve any conflicts (e.g., two files in different folders both named `Singularity` the most recent takes preference. For the above, we would build the recipe at the base of the repo, because it is dated more recently.

## Building Recipes

Once we have a list of recipe files associated with commits, the builders need to decide to create a new container, or replace an old one. The logic works as follows:

 - if a container with the same tag, name, branch, and collection is not found, this means we build, and we build a new container.
 - if containers are found with these same matching identifiers but they are frozen, we build a new container. The version is specified by both the commit id (the container's commit) and the hash (the container's hash).
 - if a previous container is found and it's *not* frozen, we replace it with the new build.
 - if a container is found and (regardless of frozen status) the tag is latest, we build over it. It doesn't make sense to freeze a "latest."

And of course all builds are subject to the [build limits](../regulatory/limits)
