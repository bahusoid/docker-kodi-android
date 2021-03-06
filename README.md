# docker-kodi-android

## General

This repository provides an easy to set up build environment for Kodi for Android.
Note: at this moment this has only been tested for the ARM platform.
Note: this was only tested on 18.0 and forth and will NOT be tested for 18 beta or RC versions.

## Versioning

This repository follows the branching scheme of the `kodi` project.

## Prerequisite

Install Docker Engine from [here](https://www.docker.com/products/docker-engine).

## Instructions

1. Run the following command to build the Docker image:
   ```
   docker build -t kodi .
   ```
   And go take a coffee or something. On my computer it takes 25 minutes.

2. Clone a fresh copy of Kodi from Github. You should probably do this parallel to step 1.
   If you checkout another version after a full build, your new build will fail at some point.
   If you know how to fix this, please do!

3. Run the following command to start a shell on the container assuming you cloned Kodi to
   `~/github/xbmc` (the ID could also just be "kodi"):
   ```
   docker run -v ~/github/xbmc:/root/kodi -it \<id_from_last_step\> /bin/bash
   ```

4. Start step 5 from [here](https://github.com/xbmc/Xbmc/blob/master/docs/README.Android.md#5-build-tools-and-dependencies)   with the following notes:
   - Add `--enable-debug=no` for a release build.

5. So, a full transcript for ARM:
   - `cd $HOME/kodi/tools/depends && ./bootstrap`
   - ```
     ./configure --with-tarballs=$HOME/android-tools/xbmc-tarballs \
       --host=arm-linux-androideabi --with-sdk-path=$HOME/android-tools/android-sdk-linux \
       --with-ndk-path=$HOME/android-tools/android-ndk-r20 \
       --prefix=$HOME/android-tools/xbmc-depends --enable-debug=no && \
       make -j$(getconf _NPROCESSORS_ONLN)
     ```
   - `cd $HOME/kodi && make -j$(getconf _NPROCESSORS_ONLN) -C tools/depends/target/binary-addons` (if it fails, launch make again)
   - `make -C tools/depends/target/cmakebuildsys && cd $HOME/kodi/build && make -j$(getconf _NPROCESSORS_ONLN) && make apk`
