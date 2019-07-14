---
title: Naming Containers
pdf: true
toc: false
---

# Referencing Containers

Singularity Hub maintains a uri scheme similar to Docker and Github to give it's containers, within the scope of a single registry, a unique uri. The convention is the following:

```bash
shub://<registry>/<username>/<reponame>:<tag>@digest
```

## Reference Components

 - `username`: maps to your Github username, or in the case of an organization repository, the organization. We use Github's namespace so we don't need to maintain our own, or manage permissions for editing the recipes.
 - `reponame`: is the name of the repository.
 - `tag`: corresponds to the extension of the Singularity file. In the old Singularity hub, it corresponded to a branch.
 - `digest`: refers to a commit id or a container (md5 sum) hash.
 - `registry`: is the host URL where the container is being served. 

For registry, if you don't have https, then when you pull the image with singularity you would need to specify to not use it:

```bash
SINGULARITY_NOHTTPS=yes singularity pull shub://vsoch/hello-world
```

A full example unique resource identifier would look like:

```bash
shub://singularity-hub.org/vsoch/hello-world:latest@<commit>
```

but since Singularity Hub (singularity-hub.org) is the default for `<registry>`, you typically don't need to specify it:

```bash
shub://<username>/<reponame>:<tag>@digest
shub://vsoch/hello-world:latest@<commit>
```

If your Singularity Registry Server is different, just say so!

```bash
$ singularity pull shub://containers-ftw.org/vsoch/hello-world
```

Regardless of how you build, each is available programatically from the command line, and for interactive viewing from the browser:

![Collection View](https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/container-builds.png)
