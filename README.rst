PhotoPrismPi
============

An out of the box `Raspberry Pi <http://www.raspberrypi.org/>`_ Raspbian distro with `PhotoPrism <https://photoprism.org/>`_ installed. 

PhotoPrismPi uses `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_, which supported building variant images and customisation.

Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/PhotoPrismPi>`_

How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``photoprismpi-wpa-supplicant.txt`` at the root of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card
#. Hostname is ``photoprismpi`` (not ``raspberrypi`` as usual), username: ``ubuntu`` and inital password is: ``ubuntu``
#. after a few mintues you should be able to access ``http://photoprismpi.local/``
#. If you attach an usb storage device it should be avilable for indexing automatically.
#. You can change the settings of photoprism in the file locate at ``/boot/firmware/docker-compose/photoprism/docker-compose.yml``


Requirements
------------
* Raspberrypi 3/3B+/3A+/4B
* 2A power supply

Features
--------

* PhotoPrisim insatalled, upgrades automatically via docker
* Automatically mountes media storages using fat32 and ext4 filesystems.


Developing
----------

Requirements
~~~~~~~~~~~~

#. Docker or Vagrant, docker recommended
#. Docker-compose - recommended if using docker build method, instructions assume you have it
#. Downloaded `Ubuntu ARM 64bit image <https://wiki.ubuntu.com/ARM/RaspberryPi/>`_ image.
#. root privileges for chroot
#. Bash
#. sudo (the script itself calls it, running as root without sudo won't work)

Build PhotoPrismPi
~~~~~~~~~~~~~~~~~~

PhotoPrismPi can be built using docker running either on an intel or RaspberryPi (supported ones listed).
Build requires about 4.5 GB of free space available.
You can build it assuming you already have docker and docker-compose installed issuing the following commands::

    
    git clone https://github.com/guysoft/PhotoPrismPi.git
    cd PhotoPrismPi/src/image
    wget -c --trust-server-names 'http://cdimage.ubuntu.com/releases/eoan/release/ubuntu-19.10.1-preinstalled-server-arm64+raspi3.img.xz'
    cd ..
    sudo docker-compose up -d
    sudo docker exec -it photoprismpi-build build
    
Building PhotoPrismPi Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PhotoPrismPi supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo docker exec -it photoprismpi-build build [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build PhotoPrismPi in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd PhotoPrismPi/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd PhotoPrismPi/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd PhotoPrismPi/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building PhotoPrismPi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
