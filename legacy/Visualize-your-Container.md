Singularity Hub generates views for you to interactively explore the guts of your container.

# Builds
Singularity provides each container build with a details view that includes the build specification file, along with calculated metrics and renderings of the build process and container contents. You are allowed one private build, and can disable your Github pushes from building at any time with <button class="btn btn-xs btn-default" disabled="">Disable</button>.

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/containers/description.png]]

Notice the pale yellow box around the description? We pull the original description from the Github Repository, and update the collection as this changes, however for individual containers you can click on the description to edit it for the container.

If the build was successful or errored with a log, it will be available for viewing:

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/containers/log.png]]

Data is also available programatically via our [API](https://www.singularity-hub.org/api).


# Compare
We have added a new view, developed for Singularity Registry, to compare sizes of contained collections across Singularity Hub:

[[https://github.com/singularityhub/singularityhub.github.io/blob/master/img/sizes.png]]

You can also see a nightly generated tree of the same collections.