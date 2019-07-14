---
title: "Builds: Manual"
pdf: true
toc: false
---

# Manual Builds

If you preference is to connect the repository and manually trigger builds, this is now possible. To deploy manually, the automatic building is disabled.

## What is a manual build?

Manual building is appropriate when you generally don't want your commits to build, and you don't maintain a workflow that uses a branch for production, and others for development. For the times when you want to build manually with a button, and by selecting any commit from your collection's previous, you can go under

```
Collection --> Settings --> Edit Builder
```

and then select "Manual." Once manual is enabled, a "Trigger Build" button will appear in your main collection view. You can click it to select the commit and recipe file that you want to build. The same rules for the queue apply - you are allowed 5 active and pending builds, and one active build at a time. You can always return to build via commit or deployment if desired.
