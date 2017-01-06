---
layout: post
title: "Setting up Kali on a ODROID-C2"
date: 2017-01-06
---
Here is how to build a pentest drop box using Kali on ODROID-C2 hardware. Im using an eMMC so this will be for that type of storage.

1. First download the ODROID-C2 image from the following URL:  
https://www.offensive-security.com/kali-linux-arm-images/
2.  Get the image onto the eMMC  
On Mac
  * Use a tool like The Unarchiver to extract the .xz downloaded file.  
  * Plug in your eMMC to the Mac.  
  * Run the following in your terminal:  
      * `df -h` This will show you where the eMMC is mounted.
      * `sudo diskutil unmount /dev/disk2s1` Unmount the eMMC.
      * `sudo dd bs=1m if=kali-2.1.2-odroidc2.img of=/dev/disk2` Copy the image to the eMMC.
3. With the ODROID-C2 there is a bug on reboot corrupting the boot uInitrd file. Here is a workaround.
  * Create a backup folder on /boot while the eMMC is still plugged into your computer.
  * Copy files (uInitrd, Image, and meson64_odroidc2.dtb) from the /boot dir into /boot/backup.
  * Plug in the eMMC into the ODROID-C2.
  * Mount the boot partition and auto mount on boot.
      * `mount /dev/mmcblk0p1 /boot`
      * `echo '/dev/mmcblk0p1 /boot auto defaults 0 0' >> /etc/fstab`
  * Create a script that copies the backup files back into /boot on start.  
      `nano /boot/backup/restore.sh`

      ```
      #!/bin/bash
      cp /boot/backup/Image /boot/
      cp /boot/backup/meson64_odroidc2.dtb /boot/
      cp /boot/backup/uInitrd /boot/
      ```

  * Set the script to executable and test it.

      ```
      chmod 755 /boot/backup/restore.sh  
      /boot/backup/restore.sh  
      ```

  * Run the script on rc.local  
  `nano /etc/rc.local`
        * Before `exit 0` place the following:  
        `/boot/backup/restore.sh`
4. Expand the filesystem to use the whole eMMC  

  ```
  fdisk /dev/mmcblk0
  d                               #delete a partition
  2                               #select partition 2
  n                               #new partition
  p                               #primary partition
  2                               #partition number 2
  Accept default First sector     #The start sector
  Accept default Last sector      #The end sector
  w                               #write the changes
  reboot                          #reboot
  resize2fs /dev/mmcblk0p2        #grow the partition
  ```

5. Upgrade the OS. Change passwords and regenerate ssh keys.  

The pentest drop box is ready for some customization. This would include getting wifi working, autossh, and other naughty things.
