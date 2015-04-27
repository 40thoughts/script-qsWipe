qsWipe
======

This script has been written to provide a simple way to wipe a device such as a disk, a partition or a USB stick.  
It also needs `cryptsetup` to run correctly, so make sure you have it installed before using the script.  
On Ubuntu machines just run `sudo apt-get update && sudo apt-get install -y cryptsetup` to install the needed software.

## Table of contents

- [Why should I use it ?](#why)
- [How does it work ?](#how)
- [What do I end with ?](#what)
- [Example](#example)

## <a name="why"></a>Why should I use it ?

* Because to fill the device with zeros is quicker but not as efficient as filling it with random data for security.
* Because to fill the device with random data using `/dev/urandom` is just so slowwwwwwwwww.
* Because doing it manually becomes a pain in the a\*\* when you have many devices to wipe.

## <a name="how"></a>How does it work ?

1. The script fill the first 10MB to ensure the device couldn't be recognized by any system.
2. It creates a temporary key.
3. `cryptsetup` will encrypt and open the device with the key.
4. The `dd` command will fill the crypted device with zeros, but since the device will be closed we won't see any zeros, just random data. That where it is clever, we used the speed of a zero-fill and got
a random result on the device.
5. `cryptsetup` will close the device.
6. And simply destroy the key wherever it has been used (device, system).

## <a name="what"></a>What do I end with ?

After the script has been run, you will end with a device which has the same name but has been totally wiped, it isn't even recognized anymore.  
So this is the part where you have to format the disk by using you favourite app (`gparted`,`fdisk`,`cfdisk`,...).

But please remember that **you just wiped the disk, so DO NOT USE THE WIPE FUNCTION of your device manager** also know as **slow format** mode.  
If you don't need to resize anything, you would probably just have to set a new type for the partition to make it readable again.

## <a name="example"></a>Example

Admitting the device you want to wipe is `/dev/sdc2`, you will have to execute : `/path/to/the/script/qsWipe /dev/sdc2`.  
**Before the first use, your will have to be sure the script can be executed,** you can execute `chmod +x /path/to/the/script/qsWipe` for that.
