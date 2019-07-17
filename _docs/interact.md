---
title: "Interact with Containers"
---

# Image Interaction

Interaction with your containers comes down to pulling them for local use.

{% include alert.html title="Important!"  content="It is essential that you <strong>pull</strong> Singularity Hub containers before you interact with an image binary directly. Singularity Hub now has strict limits for its API use, so if you run an `exec`, `shell`, or `run` directly to a `shub://` unique resource identifier, especially on a supercomputer, you will likely use up your weekly quota immediately." %}

## Singularity Commands

Deploying your workflow on the command line is as simple as using [the Singularity software](https://www.sylabs.io/guides/3.1/user-guide/). In one line, we can pull an image directly from
Singularity Hub:

```bash
$ singularity pull shub://vsoch/hello-world
```

And then interact with the container binary:

```bash
singularity run hello_world_latest.sif
singularity shell hello_world_latest.sif
singularity exec hello_world_latest.sif ls /
```

{% include alert.html type="danger" content="You should <strong>never</strong> issue any of the following commands<pre><code>singularity run shub://vsoch/hello-world
singularity shell shub://vsoch/hello-world
singularity exec shub://vsoch/hello-world ls /
</code></pre><br>
As each command will use up one download of your container, and you are limited to a weekly quota."%}


## Singularity Global Client

The [Singularity Global Client](https://singularityhub.github.io/sregistry-cli) also has [a client](https://singularityhub.github.io/sregistry-cli/client-hub) to interact with Singularity Hub, and you are also able to search or keep a record of an image, meaning a manifest.

```bash
$ sregistry pull vsoch/hello-world
```

and then pipe the image name from your database into a variable

```bash
$ container=$(sregistry get vsoch/hello-world)
$ singularity run $container
RaawwWWWWWRRRR!!
```
or use it directly

```bash
$ singularity run $(sregistry get vsoch/hello-world)
RaawwWWWWWRRRR!!
```

See the [full documentation](https://singularityhub.github.io/sregistry-cli/client-hub) for more examples.


## Examples

Let's look at a few examples for different ways we can pull images. The first generic pull will retrieve the container with the most recent modified date, across the collection:

```bash
$ singularity pull shub://vsoch/hello-world
$ singularity pull shub://doc.fish/vsoch/hello-world
Progress |===================================| 100.0% 
Done. Container is at: ./vsoch-hello-world-master.simg
```
Notice that even though branch isn't relevant for the uri, it's represented and known. If I want to retrieve the last modified (most recent) version of a specific tag:

```bash
$ singularity pull shub://vsoch/singularity-images:apps
Progress |===================================| 100.0% 
```

You can also pull a particular commit or image hash, if you know it:

```bash
$ singularity pull shub://vsoch/singularity-images:apps@9dccc60594c691c861b37d458d3e284b2f6923ea
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
