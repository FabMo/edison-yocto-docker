# edison-yocto-docker
Building an Intel Edison Yocto Linux image using docker.

This is a a [fork](https://github.com/hultqvist/edison-yocto-docker) that contains recipes for buikding the intel Edison firmware from the latest Intel official Linux Yocto Project Snapshot "[iot-devkit-yp-poky-edison-20160606.zip](https://software.intel.com/en-us/iot/hardware/edison/downloads)".

Note that this snapshot contains two patches for unfixed (as of today) problems in the official release:
  - a checksum mismatch in the "icedtea" package (see forum thread [here](https://communities.intel.com/message/434269#434269))
  - the "iotkit-comm-c" Github repository that has been removed (!) by Intel because it is no longer relevant, instead of keeping it and emptying it in order to keep receipes references intact. See [this](https://communities.intel.com/thread/108000) forum thread, grrr.

The docker container can be found [here](https://hub.docker.com/r/squonk42/edison/).
