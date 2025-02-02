# rz-community-bsp
This repository provides a basic BSP that will build a simple, usable image
for Renesas RZ reference platforms.

It is designed to allow users to use the latest upstream open source projects
and is provided as-is with no support from Renesas.

For supported and fully functional software from Renesas please use the
official BSPs provided on [renesas.com](https://renesas.com).

## Dependencies
| Name | Layers | Repository | Branch | Revision |
| --- | --- | --- | --- | --- |
| poky | meta<br>meta-poky<br>meta-yocto-bsp | https://git.yoctoproject.org/poky | kirkstone | kirkstone-4.0.14 |
| meta-arm | meta-arm-toolchain<br>meta-arm | https://git.yoctoproject.org/meta-arm | kirkstone | yocto-4.0.2 |

### Optional Dependencies
The following layers are only required when certain options are selected:

| Condition |Name | Layers | Repository | Branch | Revision |
| --- | --- | --- | --- | --- | --- |
| Mainline kernel is selected<br>via `kas/kernel/mainline.yml`<br>or `KERNEL_MAINLINE=y` in `kas menu` | meta-linux-mainline | meta-linux-mainline | https://github.com/betafive/meta-linux-mainline | main | HEAD |

## Supported Machines
| SoC | Reference Platform | Machine Name |
| --- | --- | --- |
| Renesas RZ/G2H | RZ/G2H HopeRun Evaluation Kit | hihope-rzg2h |
| Renesas RZ/G2L | RZ/G2L SMARC Evaluation Kit | smarc-rzg2l |

## Provided Images
| Image Name | Description | Key Features |
| --- | --- | --- |
| renesas-image-minimal | Provides a basic image based on Poky's core-image. | Linux kernel<br>U-Boot<br>Trusted-Firmware-A |

## Building
This project is set up to be built with [Kas](https://github.com/siemens/kas).
The Kas tool provides an easy mechanism to setup bitbake based projects.
Documentation for Kas is available on [readthedocs.io](
https://kas.readthedocs.io/en/latest/userguide.html).

The build can be configured using a number of options provided in different kas
yaml files. They can be daisy chained onto each other so that multple options
can be selected.

**kas/base.yml**\
This file adds in the base configuration used by this project. It is
automatically included by the yocto version yaml files (see below).

**kas/yocto/*.yml**\
This includes the dependency configuration for building the rz-community-bsp
based on specific versions of Yocto.

**kas/machine/*.yml**\
A selection of files to use to specify which machine to build for.

**kas/image/*.yml**\
A selection of files to use to specify which image to build.

**kas/kernel/*.yml**\
A selection of files to use to specify which Linux kernel version to build.

**kas/u-boot/*.yml**\
A selection of files to use to specify which U-Boot version to build.

**kas/trusted-firmware-a/*.yml**\
A selection of files to use to specify which Trusted-Firmware-A version to build.

**kas/opt/gitlab-ci-cache.yml**\
A special option that can be used to configure the sstate and downloads
directory locations so that they can be used easily with GitLab CI/CD caching.

**kas/opt/debug.yml**\
Add various features to the image to help with development and debugging.\
Specifically, it enables the *debug-tweaks* image feature that allows users to\
be able to login without a password. More information can be found\
[here](https://docs.yoctoproject.org/dev/ref-manual/features.html#:~:text=debug%2Dtweaks%3A%20Makes%20an%20image,enables%20post%2Dinstallation%20logging).

### kas-container
The easiest way to use Kas is to use `kas-container`. This script will run the
Bitbake built in a pre-configured Docker container.

For ease of use a copy of the script has been included in this repository.

Example kas usage:
```bash
# Make sure we're inside rz-community-bsp (this repository)
cd rz-community-bsp

# Yocto: kirkstone
# Image: renesas-image-minimal
# Machine: hihope-rzg2h
# Linux: linux-renesas v5.10
./kas-container build kas/yocto/kirkstone.yml:kas/opt/debug.yml:kas/image/renesas-image-minimal.yml:kas/machine/hihope-rzg2h.yml:kas/kernel/renesas-5.10.yml

# Yocto: kirkstone
# Image: renesas-image-minimal
# Machine: smarc-rzg2l
# Linux: linux-cip v6.1
./kas-container build kas/yocto/kirkstone.yml:kas/opt/debug.yml:kas/image/renesas-image-minimal.yml:kas/machine/smarc-rzg2l.yml:kas/kernel/cip-6.1.yml

# If you are re-building in the same directory as a previous build it may be
# prudent to use --update and --force-checkout to ensure that the dependency
# repositories are correct
./kas-container build --update --force-checkout kas/yocto/kirkstone.yml:kas/opt/debug.yml:kas/image/renesas-image-minimal.yml:kas/machine/hihope-rzg2h.yml:kas/kernel/cip-6.1.yml
```

## Kas Menu
Kas supports a Kconfig based menu system which can be accessed with the
`kas menu` (or `kas-container menu`) command. This allows users to easily select
between options without having to manually select the kas yaml files required
for their build.

Follow the instuctions in the menu to select the build configuration and then
either "Save & Build" to save the configuration to a .config.yaml file and kick
off a build straight way, or "Save & Exit" to only store the configuration to a
.config.yaml file for future use.

# Contributions
Contributions are *very* welcome! Please submit a pull request for review.

# License
This repository and its contents is licensed under MIT. See
[COPYING.MIT](COPYING.MIT) for details.

# Maintenance
This repository provided on a best-effort basis. There are no promises that
everything will work!

Maintainer: **Chris Paterson**
* Email: [chris.paterson2@renesas.com](mailto:chris.paterson2@renesas.com)
* GitHub/GitLab username: patersonc
* IRC (Libera.Chat): patersonc
