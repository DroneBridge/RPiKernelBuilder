[![Actions Status](https://github.com/webbbn/docker-rpi3-kernel-builder/workflows/build/badge.svg)

# Docker Raspberry Pi kernel builder for [Open.HD](https://github.com/HD-Fpv/Open.HD)

Docker image to build a Raspberry Pi kernel with patches required for OpenHD

## Preparation

This builder uses docker in order to help isolate the build process from the host OS and make the process more repeatable and easier to perform on a wider range of platforms.

If you're on an Ubuntu or other Debian-based system, you can install docker using the following command:

~~~
sudo apt-get update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
~~~

The build script assumes that quemu-arm-static will be installed. That can be installed on Ubuntu/debian using:

~~~
sudo apt-get install qemu-user-static
~~~

For other systems you can find installation instructions at: https://docs.docker.com/install/overview/

## Building

A script is provided that will build the appropriate docker container and build both a armv6 and armv7 kernel.

The kernels will be in the build directory after building is complete.

## Attribution
Based on:
- https://github.com/simonvanderveldt/docker-rpi3-kernel-builder
- https://github.com/DieterReuter/rpi64-kernel
- https://autostatic.com/2017/06/27/rpi-3-and-the-real-time-kernel/
