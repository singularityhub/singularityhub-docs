When you build an image, you will notice several metrics are recorded:

## Build Metrics

 - `tag`: corresponds to the extension of the `Singularity` file it was built from, regardless of the folder in the repository.
 - `branch`: refers to the branch of the repository from which the container was built. Master is enabled by default, and you are required to enable other branches in your collection settings.
 - `singularity/singularity-python version`: a record of the Singularity and Singularity Python versions that were used for your build
 - `version`: refers to the file hash of the image. For the old Singularity Hub, the version was the commit, and this is no longer the case. This is considered one of the "digest" options if you want to reference it in a URI for a pull.
 - `commit`: refers to the Github commit associated with your image. This is also considered one of the "digest" options if you want to reference it in a URI for a pull.
 - `size`: The size, in MB, of your finished container.
 - `estimated_os`: takes the entire file hierarchy of your image and compares to the base operating systems provided by Docker, and matches to the highest score. It tends to be the case that distributions are very similar (e.g., Ubuntu 14.04 vs. 16.04) and produce equivalent scores, and it's just a matter of ordering to which it is matched.
 - `extensions`: is the count of extensions when an estimated operating system is subtracted from your image.
 - `build_time`: is the number of seconds the builder took to build your image, not including time to configure, deploy, upload data to storage, and bring down the builder.

## Tags
A collection can have tags, just like a container, however these "tags" are more properly Github topics that are derived directly from your Github repository. These are the little bubbles (akin to tags) that appear right above the Code view. The tags make it easy to (in the long run) find common containers based on human annotations. If you are a collection owner, you can also manually click the bar of tags to add them.

## Descriptions
A description for the collection is derived (and updated) from the Github repository, and given to each container on a new build. If you are the collection owner, however, you can click on the description for a specific container build to customize it, and it saves automatically. It will not be changed from your edits.

## Apps
Apps (or Standard Container Integration Format, SCI-F Apps) are internal modules that are discovered in your container, if you create them using [SCI-F](https://containers-ftw.github.io/SCI-F/).