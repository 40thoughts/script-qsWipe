qsWipe
======

This script has been written to provide a simple way to wipe a device such as a disk, a partition or a USB stick.  
It also needs `cryptsetup` to run correctly, so make sure you have it installed before using the script.  
On Ubuntu machines just run `sudo apt-get update && sudo apt-get install -y cryptsetup` to install the needed software.

## Table of contents

- [Why should I use it ?](#why-should-i-use-it-)
- [How does it work ?](#how-does-it-work-)
- [What do I end up with ?](#what-do-i-end-up-with-)
- [Example](#example)
- [Licensing](#licensing)

## Why should I use it ?

* Because to fill the device with zeros is quicker but not as efficient as filling it with random data for security.
* Because to fill the device with random data using `/dev/urandom` is just so slowwwwwwwwww.
* Because doing it manually becomes a pain in the a\*\* when you have many devices to wipe.

## How does it work ?

1. The script wipe the first 10MB to ensure the device couldn't be recognized and mounted by the system.
2. It creates a temporary key.
3. `cryptsetup` will encrypt and open the device with the key.
4. The `dd` command will fill the crypted device with zeros, but since the device will be closed we won't see any zeros, just random data. That's where it becomes clever, we used the speed of a zero-fill and got a random result on the device.
5. `cryptsetup` will close the device.
6. And simply destroy the key wherever it has been used (device, system).

## What do I end up with ?

After the script has been run, you will end up with a device which has the same name but all the space on it has been totally wiped, it isn't even recognized anymore.  
So this is the part where you have to format the disk by using you favourite app (`gparted`,`fdisk`,`cfdisk`,...).

But please remember that **you just wiped the disk, so DO NOT USE THE WIPE FUNCTION of your device manager** also know as **slow format** mode.  
If you don't need to resize anything, you would probably just have to set a new type for the partition to make it readable again.

## Example

Admitting the device you want to wipe is `/dev/sdc2`, you will have to execute : `/path/to/the/script/qsWipe /dev/sdc2`.  
**Before the first use, your will have to be sure the script can be executed,** you can execute `chmod +x /path/to/the/script/qsWipe` for that.

## Licensing

Please see the file called LICENSE.
