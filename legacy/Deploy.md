## Singularity Commands

Deploying your workflow on the command line is as simple as using [the Singularity software](http://singularity.lbl.gov). In one line, we can interact with an image directly from Singularity Hub:

```bash
singularity shell shub://vsoch/hello-world
singularity run shub://vsoch/hello-world
singularity pull shub://vsoch/hello-world
singularity exec shub://vsoch/hello-world ls /
```

As of Singularity 2.4, you can also use a Singularity Hub image as your Build base. This would be the header of your Singularity Recipe:

```
From: vsoch/hello-world
Bootstrap: shub
```
and then given your `Singularity` Recipe file (above), you would build as follows:

```
sudo singularity build container.simg Singularity
```

## Singularity Global Client

The [Singularity Global Client](https://singularityhub.github.io/sregistry-cli) also has [a client](https://singularityhub.github.io/sregistry-cli/client-hub) to interact with Singularity Hub, and you are also able to search or keep a record of an image, meaning a manifest.

```bash
sregistry pull vsoch/hello-world
sregistry search vsoch/hello-world
sregistry record vsoch/hello-world
```

and then pipe the image name from your database into a variable

```
container=$(sregistry get vsoch/hello-world)
singularity run $container
RaawwWWWWWRRRR!!
```
or use it directly

```
singularity run $(sregistry get vsoch/hello-world)
RaawwWWWWWRRRR!!
```

See the [full documentation](https://singularityhub.github.io/sregistry-cli/client-hub) for more examples.

## Tags and Branches
Remember that a single Github repository is called a "Collection," and is associated with multiple container builds, and there can be more than one build per commit if more than one Singularity Recipe are present with different extensions, each indicating a tag:

```
Singularity       <-- vsoch/hello-world:latest
Singularity.gpu   <-- vsoch/hello-world:gpu
```

By default, we build from the `master` branch when you first connect your repository, and if you want to add another branch to build, you can do this in your collection view:

```
Collection --> Settings --> Edit Branches
```

You can then turn on (and off) branches to build. This is also helpful if you want to disable a branch for building, while testing, or something like that:

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/collection-branches-details.png]]

Branches are a useful way for allowing automated building without having building happen for every single commit. If you have your production branch enabled for building, it will only build final images when you commit to it.

## Examples
Let's look at a few examples for different ways we can pull images. The first generic pull will retrieve the container with the most recent modified date, across the collection:

```
singularity pull shub://vsoch/hello-world
singularity pull shub://doc.fish/vsoch/hello-world
Progress |===================================| 100.0% 
Done. Container is at: ./vsoch-hello-world-master.simg
```
Notice that even though branch isn't relevant for the uri, it's represented and known. If I want to retrieve the last modified (most recent) version of a specific tag:

```
singularity pull shub://vsoch/singularity-images:apps
Progress |===================================| 100.0% 
```

You can also pull a particular commit or image hash, if you know it:

```
singularity pull shub://vsoch/singularity-images:apps@9dccc60594c691c861b37d458d3e284b2f6923ea
Progress |===================================| 100.0% 
```

## Order of Operations
Singularity Hub allows some flexibility in asking for containers. It takes the following order of operations into account:

 1. **shub://vsoch/hello-world** Is the most general query, and returns the most recent image for the collection. This coincides with the tag `latest`. 
 2. **shub://vsoch/hello-world:<tag>** means that you want a particular tag. Asking for **latest** is the equivalent of `1` above.
 3. **shub://vsoch/hello-world:<tag>@<digest>** is where it get's interesting. A digest is flexible to being the following, in the order of preference:
  - **hash** the most specific kind of digest is the hash of the file. This is checked for first.
  - **commit** the next most specific is the commit
  - **branch** finally, if a matching hash and commit are not found, we fall back to the branch.

Since it's possible to have equivalent commits and branches across images, you are not allowed to ask for either without a tag. For frozen images that might have newer images built since, we recommend to you (and your users if you are an administrator) to ask for them specifically by hash. Now let's review some examples of how to pull and specify a name for a container from Singularity Hub.

## Naming your Container
Singularity Hub supports naming images by hash, commit, or a custom name. We recommend naming by the file hash since a commit can belong to multiple containers, but it's up to your preference and how you use the naming. Note that the software needs to be updated to have proper functionality with the new Singularity Hub, and you can see [the pull request](https://github.com/singularityware/singularity/pull/1059) with details for examples.

### Default Human Friendly Name
By default, your container will be named with the format `<username>-<container>-<branch>.simg`. This is expected to change (it was an oversight to not update it to include a tag with Singularity 2.4) to represent the branch *and* tag (`<username><container><branch><tag>.simg`).

### Name by Hash
To name your image by it's md5sum content hash (recommended), you can do the following:

```
singularity pull --hash shub://doc.fish/singularityhub/hello-registry:14.04@1cf54e8f76cab067b821b4c3d5542cb6
Progress |===================================| 100.0% 
Done. Container is at: /home/vanessa/1cf54e8f76cab067b821b4c3d5542cb6.simg
```

Note that the hash `1cf54e8f76cab067b821b4c3d5542cb6` is equivalent to the file name. Is it really the hash?

```
md5sum 1cf54e8f76cab067b821b4c3d5542cb6.simg 
1cf54e8f76cab067b821b4c3d5542cb6  1cf54e8f76cab067b821b4c3d5542cb6.simg
```

If we ask for the same file based on a commit `d7cb6fa8c4f45f6ba589ce6c0c2a78c4e93225ee`, we still get the container with the correct hash:

```
singularity pull --hash shub://doc.fish/singularityhub/hello-registry:14.04@d7cb6fa8c4f45f6ba589ce6c0c2a78c4e93225ee
Progress |===================================| 100.0% 
Done. Container is at: /home/vanessa/1cf54e8f76cab067b821b4c3d5542cb6.simg
```

### Name by Commit
You can also name by the Github commit, which is nice to go back and find it again. Note that this command also needs to be updated in Singularity proper, and this command will return the hash instead of the commit (a bug) as of now.

```
singularity pull --commit shub://vsoch/hello-world
Progress |===================================| 100.0% 
Done. Container is at: ./e279432e6d3962777bb7b5e8d54f30f4347d867e.img
```

### Custom name
Finally, you just might want to name it something entirely different. You can do that with `--name`:

```
singularity pull --name meatballs.img shub://vsoch/hello-world
Progress |===================================| 100.0% 
Done. Container is at: ./meatballs.img
```