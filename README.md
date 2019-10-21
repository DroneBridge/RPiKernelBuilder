# Docker Raspberry Pi 3 kernel builder [![Docker Build Status](https://img.shields.io/docker/build/simonvanderveldt/rpi3-kernel-builder.svg)](https://hub.docker.com/r/simonvanderveldt/rpi3-kernel-builder/)
Docker image to build a Raspberry Pi 3 kernel for the `arm` architecture

## How to use
Building a kernel using the rpi3-kernel-builder Docker image works according to the following conventions.
Configuration is done using environment variables:
- The kernel sources should be made available at `/workdir`
- The build will output to `/workdir/build`
- The output is a file called `<kernelversion>.tar.gz` which contains:
  - The kernel
  - The kernel modules
  - The kernel firmware
  - Overlays
  - `config.txt` if available in the sources
  - `cmdline.txt` if available in the sources
  - `dt-blob.bin` if `dt-blob.dts` is available in the sources
- To apply patches to the kernel sources make the patches available at `PATCH_DIRS`. This is a space separated array of paths.
The patches need to have the extension `.patch`
- The kernel can be configured using a `defconfig` or using `all*config`. The default is to configure using `bcm2709_defconfig`
  - To override the default `defconfig` set `DEFCONFIG` to the `defconfig`'s name and make sure the `defconfig` is made available at the correct path.
  For example `DEFCONFIG=my_defconfig` with `my_defconfig` being available at `arch/arm/configs/my_defconfig`'
  - To configure using `all*config` set `ALLCONFIG` to one of the allconfig options `allyesconfig`, `allmodconfig`, `allnoconfig` or `randconfig`.
  To override which config is used as the starting point set [`KCONFIG_ALLCONFIG`](https://github.com/raspberrypi/linux/blob/560909d433109e3da08757237f30576c71697914/Documentation/kbuild/kconfig.txt#L51) to the path of the config file. For example `ALLCONFIG=/workdir/configs/myminimalconfig`
- The amount of jobs used by make will default to the value returned by `nproc`.
  To override this set the `MAKEFLAGS` environment variable. For example `MAKEFLAGS=-j8`.

### Example usage
```
# Checkout kernel sources
$ git clone --single-branch --branch rpi-4.14.y --depth 1 https://www.github.com/raspberrypi/linux

# Build a kernel archive using the docker image
# using a volume mount for the linux sources to /workdir
$ cd linux
$ docker run --rm -ti -v "${PWD}":/workdir simonvanderveldt/rpi3-kernel-builder

# When the build is done the kernel archive is available in the build directory
$ ls -ahl build/
total 28M
drwxr-xr-x 2 simon simon    6 Feb  3 23:21 .
drwxr-xr-x 5 simon simon    6 Feb  3 23:08 ..
-rw-r--r-- 1 root  root  178K Feb  3 23:21 kernel-4.9.59-0.0.7-g8172bbc.sha256
-rw-r--r-- 1 root  root   11M Feb  3 23:21 kernel-4.9.59-0.0.7-g8172bbc.tar.gz
```

## Attribution
Based on:
- https://github.com/DieterReuter/rpi64-kernel
- https://autostatic.com/2017/06/27/rpi-3-and-the-real-time-kernel/
