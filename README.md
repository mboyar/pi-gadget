# Raspberry Pi "Universal" Gadget Snap

This repository contains the source for an Ubuntu Core gadget snap that runs
universally on all Raspberry Pi 3 boards currently supported by Ubuntu Core
(the Raspberry Pi 2B, 3B, 3A+, 3B+, 4B, Compute Module 3, and Compute Module
3+).

Building it with snapcraft will obtain various components from the
bionic-updates archive, including:

* the bootloader firmware from the linux-firmware-raspi2 package
* the device-tree(s) from the linux-modules-<ver>-raspi2 package
* u-boot binaries (for various models) from the u-boot-rpi package
* u-boot boot script from the flash-kernel package (classic gadgets only)

On core builds, a silent boot with a splash screen is included. The splash
screen binary comes from git://git.yoctoproject.org/psplash. Please see the
`psplash/` sub directory for patches and adjustments in use.


## Gadget Snaps

Gadget snaps are a special type of snaps that contain device specific support
code and data. You can read more about them in the official docs:
https://ubuntu.com/core/docs/gadget-snaps
https://ubuntu.com/core/docs/gadget-building


## Reporting Issues

Please report all issues here on the github page via:
https://github.com/mboyar/pi-gadget/issues


## Branding

This gadget snap comes with a boot splash. To change the logo you can add a new
png file to the psplash subdirectory of this tree, adjust the "SPLASH=" option
in `psplash/config` to point to this file and rebuild the gadget.

To turn off the splash screen completely please edit `configs/core/cmdline.txt`
and remove the `splash` and the `vt.handoff=2` keywords from the default kernel
commandline.


## Building for Raspberry Pi 2

This gadget snap can be cross built on a VM (multipass). To do so,
just run `snapcraft` without any arguments in the top level of the source tree after cloning it and selecting the appropriate branch:   

    $ sudo snap install snapcraft --classic
    $ git clone https://github.com/mboyar/pi-gadget
    $ cd pi-gadget
    $ git checkout 20-armhf

After manipulating the files you want, continue to apply the steps below to create the gadget snap and then the pi image;

    $ sudo snapcraft clean
    $ snapcraft
    $ snap known --remote model series=16 model=ubuntu-core-20-pi-armhf-dangerous  brand-id=canonical >ubuntu-core-20-pi-armhf-dangerous.model
    $ sudo ubuntu-image --channel stable --snap pi_20-1_armhf.snap ubuntu-core-20-pi-armhf-dangerous.model
    $ file pi.img 
    pi.img: DOS/MBR boot sector; partition 1 : ID=0xc, start-CHS (0x10,0,1), end-CHS (0x3ff,3,32), startsector 2048, 2457600 sectors


Now, you can burn the `pi.img` to a micro SDcard (>4GB, Class10 or SDHC) by using your favorite disk imager. Follow the steps in the URL https://ubuntu.com/download/raspberry-pi-core (skip **Step 2** to use your Raspberry Pi with your own image `pi.img`)



