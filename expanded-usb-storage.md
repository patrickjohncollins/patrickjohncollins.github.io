Title: My ever-expanding data storage needs
Date: 30-12-2015
Slug: expanded-usb-storage

[My server](/server/) only has an 8Gb SD card, which is a bit tight for all the data I'd like to store on it.  I consulted with my [hosting company](/host/) and they suggested a USB memory stick of tiny dimensions.  I ordered a [Sandisk Ultra Fit](https://www.sandisk.com/home/usb-flash/ultra-fit-usb) 128 GB USB memory stick through Amazon, which set me back €50, and had it delivered directly to my host, which they plugged in without issue.

### Magical incantations to reassign bits

The memory stick was supplied with the FAT32 file system, which is actually a Microsoft Windows invention.  However, my server is running Linux so it seemed like a good idea to switch to the ext4 file system.  I don't know if this was worth doing.  Here are the commands I used:

	sudo fdisk /dev/sda

	Welcome to fdisk (util-linux 2.25.2).
	Changes will remain in memory only, until you decide to write them.
	Be careful before using the write command.


	Command (m for help): t
	Selected partition 1
	Hex code (type L to list all codes): 83
	If you have created or modified any DOS 6.x partitions, please see the fdisk documentation for additional information.
	Changed type of partition 'W95 FAT32 (LBA)' to 'Linux'.

	Command (m for help): w
	The partition table has been altered.
	Calling ioctl() to re-read partition table.

Then, I decided to format the drive as well:

	sudo mkfs.ext4 /dev/sda1 -L untitled

### Magical incantations to access the extended storage

I created a new directory:

	sudo mkdir /media/usbhdd
	sudo chown root:root /media/usbhdd

And then mounted the memory stick:

	sudo mount /dev/sda1 /media/usbhdd

To ensure that the memory stick would be mounted at boot, I edited the fstab file:

	sudo nano /etc/fstab

Where I added the following line to the end:

	/dev/sda1       /media/usbhdd   ext4    defaults        0       0

I then rebooted the machine to test:

	sudo reboot
	