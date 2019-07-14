---
title: Build Settings
---

# Build Settings

The Build Settings page includes builder options, branch activation, along with administrative actions for your container collection. The page can be accessed by clicking on "Settings" from the collection view.

## Builder Settings

The builder settings panel allows you to choose the builder image (the size of the instance and version of Singularity), if you want debug output (more verbosity) at build time, and the kind of build trigger (commit, manual, or deploy) that is desired for your collection.

![Build Settings](https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/settings/settings_builder.png)

___

## Branch Settings
This panel will display all available branches for your collection, and allow you to turn them on and off. A branch that is off will not build. This option is useful if you intend to commit many times and don't want to trigger redundant builds, or to enable a particular branch that is considered a production branch.

![Build Settings](https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/settings/settings_branches.png)

___

## Administrative Settings

This is a **danger zone!** Here is where you can disable building entirely, make the collection private, or delete the collection entirely. Be careful with deletion as it means that all of the collection's containers and metadata will be deleted. This action cannot be undone.

![Build Settings](https://raw.githubusercontent.com/singularityhub/singularityhub.github.io/master/img/settings/settings_admin.png)

