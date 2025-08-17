## **Actions**
![Version](https://img.shields.io/badge/version-352706903455cebc260fd565a38708c0e6423dc7-blue)
![Build Status](https://codebuild.us-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiQkRkY0htL2tWTlM0QmVLSS9SakxYOHBRTUxJNmczcVM4Nm1Wa0U1TFQvVkRDTHRadys0aEVIOStxc0V4aGxSQzNsdVZlaXV5R1YvaHZaOUZIZnRTTWtzPSIsIml2UGFyYW1ldGVyU3BlYyI6ImZjajIxazcybkxaZVdUR24iLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=main)

[Actions](https://github.com/tinkerbell/actions) is the repository that contains reusable Tinkerbell Actions.

### Updating

1. Review commits upstream [repo](https://github.com/tinkerbell/actions) and decide on release tag to track.
1. Follow these steps for changes to the patches/ folder:
    1. Checkout the desired tag on upstream [repo](https://github.com/kubernetes-sigs/image-builder) and create a new branch on your local workspace.
    1. Review the patches under patches/ folder in this repo. Apply the required patches to the new branch in the local clone of upstream repo created in the above step.
        1. Run `git am <path to patches>` on the upstream clone.
        1. For patches that need some manual changes, you will see a similar error: `Patch failed at *`
        1. For that patch, run `git apply --reject --whitespace=fix *.patch`. This will apply hunks of the patch that do apply correctly, leaving
           the failing parts in a new file ending in `.rej`. This file shows what changes weren't applied and you need to manually apply.
        1. Once the changes are done, delete the `.rej` file and run `git add .` and `git am --continue`
    1. Remove any patches that are either merged upstream or no longer needed. Please reach out to @jaxesn, @vignesh-goutham or @g-gaston if there are any questions regarding keeping/removing patches.
    1. Run `git format-patch <commit>`, where `<commit>` is the last upstream commit on that tag. Move the generated patches from under the upstream fork to the projects/tinkerbell/actions/patches/ folder in this repo.
1. Update the `GIT_TAG` file to have the new desired tag based on upstream.
1. Verify the golang version has not changed. Currently the version 1.24 mentioned in the [Dockerfile](https://github.com/tinkerbell/actions/blob/main/cexec/Dockerfile) of each action.
1. Verify no changes have been made to the Dockerfile for each action looking specifically for added dependencies or build 
process changes.
1. Update checksums and attribution using `make attribution checksums`.
1. Update the version at the top of this Readme.
1. Run `make generate` to update the UPSTREAM_PROJECTS.yaml file.
