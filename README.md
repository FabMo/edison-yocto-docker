# edison-yocto-docker
Building an Intel Edison Yocto Linux image using docker.

This is a a [fork](https://github.com/hultqvist/edison-yocto-docker) that contains docker recipes for building the intel Edison firmware from the latest sources in Intel official Linux Yocto Project Snapshot "[iot-devkit-yp-poky-edison-20160606.zip](https://software.intel.com/en-us/iot/hardware/edison/downloads)".

Note that this repository contains two patches for unfixed (as of today) problems in the official release:
  - a checksum mismatch in the "icedtea" package (see forum thread [here](https://communities.intel.com/message/434269#434269) and mailing list [here](https://lists.yoctoproject.org/pipermail/yocto/2016-October/032374.html))
  - the "iotkit-comm-c" Github repository that has been removed (!) by Intel because it is no longer relevant, instead of keeping it and emptying it in order to keep receipes references intact. See [this](https://communities.intel.com/thread/108000) forum thread, grrr.

The docker container can be found [here](https://hub.docker.com/r/squonk42/edison/).

Background
==========

The Intel Edison firmwares are created using the [Yocto Project](https://www.yoctoproject.org/) build system. Yocto is not yet another Linux distribution, it is a distribution build system, based on [OpenEmbedded tools](http://www.openembedded.org/wiki/Main_Page).

In OpenEmbedded (and thus Yocto), a layer is just a collection of recipes and/or configuration that can be used on top of [OE-Core](http://www.openembedded.org/wiki/OpenEmbedded-Core). Typically each layer is organised around a specific theme, e.g. adding recipes for building web browser software.

This build system is designed to run on any Linux host distro and contains today 7500 recipes, covering approximately 300 machines and 20 distros.

Unfortunately, Yocto does not build correctly on modern Linux distros using GCC 5, this includes the latest Linux Ubuntu 16.04 LTS and Linux Mint 18. The only way to get it to compile correctly is to use an older version, like Linux Ubuntu 14.04 LTS, but reverting your devel machine to this version is probably not a good idea :-)

At first glance, a good alternative would be to run the Linux Ubuntu 14.04 LTS into a virtual machine, using either VirtualBox or VMWare. However, this solution takes a lot of time to setup the machine, uses disk space to store a full-fledged image with all bells and whistles, and resources to run 2 OSes using Hyperthreading.

This is where Docker is proving helpful: instead of virtualizing a whole machine, docker only virtualizes a given process, offering it an emulated host environment to comfortably run into. And as docker is also able to journalize the underlying emulated machine into successive layers which act much like git commits and provides a network bridge to the external world, it becomes easy to write deterministic docker scripts to build docker images for any dedicated task, being it a web server or an embedded distro building machine in our case.

The idea is then to use docker to fetch a base Linux 14.04 LTS image and add docker recipes to fetch all required building tools and required packages in order to generate the Yocto Edison images into it. The final images can then be exported to your host machine for eventually flashing it into the Intel Edison module.

Requirements
============

If not already available, you will have to install the Docker Engine onto your host machine (Linux, Windows or Mac), following [these instructions](https://docs.docker.com/engine/installation/).

You will need 53+ GB of available space in order to store the docker images containing all deflated source packages and generated binary files. In case the available space is located onto a different partition, you may have to specify to docker where to store its images (instructions for [Linux](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169/20), [Windows](https://forums.docker.com/t/where-are-images-stored/9794) and [Mac](https://forums.docker.com/t/change-docker-image-directory-for-mac/18891)).

Usage
=====

Clone this repository using git:

    $ git clone https://github.com/Squonk42/edison-yocto-docker.git

Alternatively, you could just grab the .zip archive containing the latest files [here](https://github.com/Squonk42/edison-yocto-docker/archive/master.zip), unzip it and renamed the "edison-yocto-docker-master" to "edison-yocto_docker".

Then run the build script using:

    $ cd edison-yocto-docker
    $ source build.sh

This should take ~5-6 hours on a modern Core i7 CPU with good Internet connection and a mechanical HD, faster if using an SSD drive, but you have time to take your favorite beverage.

Eventually, you should be rewarded by an "All done" message.

In order to retrieve the resulting binary file to Flash into the Edison module, please run the "squonk42/edison" image:

    $ docker run squonk42/edison

Find out its container ID by listing the running processes:

    $ docker ps -a

Copy the file out of the container onto your local directory:

    $ docker cp CONTAINERID:/home/edison/toFlash.zip .

Now you will need to figure out how to modify the generated Yocto Edison distro!
