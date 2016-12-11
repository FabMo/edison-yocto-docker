# edison-yocto-docker
Building an Intel Edison Yocto Linux image using docker.

This is a a [fork](https://github.com/hultqvist/edison-yocto-docker) that contains recipes for buikding the intel Edison firmware from the latest Intel official Linux Yocto Project Snapshot "[iot-devkit-yp-poky-edison-20160606.zip](https://software.intel.com/en-us/iot/hardware/edison/downloads)".

Note that these snapshot contains at least a checksum mismatch in the icedtea package (see forum thread [here](https://communities.intel.com/message/434269#434269)), so only a reduced working set is included (see [here](https://communities.intel.com/message/431971#431971)).
