---
title: Managing Containers
pdf: true
toc: false
---

# Managing Containers

## Freezing

You likely want control to tag and "freeze" an image to ensure that it is available and not removed. You can do this via the Singularity Hub collection view.

## Ephemeral Images

By default, a container unique resource identifier is ephemeral in that if you commit or deploy
again, it will replace a previous version. To maintain versions of your images, along
with specifying a tag, it's recommended that you freeze all images that you want
to keep an exact version for.

For example, if you push an image with tag `gpu` and then push another commit, the previous image will be replaced. This change addresses the observation for early Singularity Hub that many users would not clean up redundant builds. After freezing a specific version, any subsequent pushes with a Singularity Recipe that would have over-ridden an image will in fact produce a new image file, the same uri but referenced by a different commit and version:

```bash
shub://vsoch/hello-world:gpu@123  (-- frozen
shub://vsoch/hello-world:gpu@234
```

If gpu commit `123` was not frozen, it would completely be replaced by commit `234`. You are free to freeze as many images as you like. Frozen images cannot be edited or changed, as it is an assurance of being just that. Frozen.

See [best practies](../regulatory/best-practices) for building to help ensure you are
building responsibly.
