# archlinux-lts-zfs

An automated custom archiso build with a small number of changes compared to the official iso. The approach is based on the work of Arch user LenHuppe's [bash script](https://bbs.archlinux.org/viewtopic.php?id=266385) to create custom archisos.

## Why use this image?

Installing Arch on ZFS ([see Arch Wiki](https://wiki.archlinux.org/title/Install_Arch_Linux_on_ZFS#Get_ZFS_module_on_archiso_system)) has a hard requirement on installing openzfs packages onto a booted archiso.
To make that process easier, tools [eoli3n/archiso-zfs](https://github.com/eoli3n/archiso-zfs) exist. That method however fails when any of the below occours:

- OpenZFS has not yet released a package for the current standard linux kernel shipped with Arch
- OpenZFS installed via dkms is not yet compatible or has ongoing compatibility issues with the standard kernel shipped by Arch
- The maintainer of the archzfs repository - which made OpenZFS releases available on Arch - disappears
- Tools like archiso-zfs are not updated in time to address any of the above
- An archiso with the LTS kernel is required, which will safeguard against ZFS issues on the most recent standard kernel


## What are the changes compared to the official iso?

The following changes are applied during image build:

- The archzfs repository (currently trageting the experimental new releases on GitHub) is added to the image
- `linux-lts`, `linux-lts-headers` and `zfs-linux-lts` packages are added to the image
- `linux`, the standard kernel package, is **not** installed
- `broadcom-wl` and `b43-fwcutter` packages are removed because they depend on the standard linux kernel package
- `PreLoader` and `HashTool` (signed) are added to the iso to support SecureBoot
- The message of the day (`motd`) is modified to call out the OpenZFS support of the iso


## How is the image build?

Creating this image follows a number of steps, namely:

1. Boot into an Arch Linux Docker image (`archlinux:latest`)
2. Install `archiso` package, which provides the official `mkarchiso` script
3. Modify the official archiso's `releng` image profile with the changes listed above
4. Run `mkarchiso` to create a custom archiso file
5. Modify the iso by including signed `PreLoader` and `HashTool` binaries
6. Save the sha256 checksum of the final iso to a file
7. Create a release on GitHub and push the iso and sha256 files into it

All of this is done inside a GitHub Action Workflow and can hence be audited.

## When is this iso updated?

A new release will automatically get created on the **4th day of every month** at 4.30 AM (Standard GitHub time).

Reasons for this delay primarily revolve around the unpredictablity of openzfs releases as well as standard-kernel compatibility issues.
Choosing to release a few days after the official archiso gets refreshed also ensures Arch mirrors have had time to sync repositories.

## Contributions

Contributions to this repository are welcome. Please do however create a GitHub issue first so we may discuss what needs to be done.
PRs will be reviewed on a best-effort basis and priority will be given to those PRs that were discussed first.