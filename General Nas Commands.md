	# General Shell commands

Change directory

```
cd folderName
```

Go to your home directory

```
cd ~
```

Change directory based on folders location from base folder: 

```
cd /folder/subfolder
```

Change from a child directory to its parent

```
cd ../folderName
```

Change from a child to another child of the same parent

```
cd ../folderName
```

Make directory

```
mkdir location/of/new/folder
```

Make multiple directories

```
mkdir location/of/new/folder location/of/second/new/folder
```

Move a file/folder or rename a file/folder


```
mv current/file/location new/file/location
```

To copy a file

```
cp file duplicateFile 
```


To copy a folder

```
cp -r /folder/location /duplicate/folder/location 
```


# NAS Notes

### Plugin setup
##### Create a folder to hold all the jails (only done once)
First you need to create a directory where you want the jails to live:

- cd  /where/you/want
- mkdir folderName

Then in the UI:

- Select jails from the top menu
- click `configuration` (the fourth tab)
- click `advanced` 
- select folder just created
- set `IPv4 Network Start Address:` to the address you wish to start at.

### Add a plugin
From the top menu, select `plugins` and select the desired plugin from `available` and install

### Create plugins folder sharing
I have had more success manually creating folders than relying on the ui. Enter shell:

- `jls` (get a `ls` of jails ) and get the id of the jail you want
- `jexec <jailid> /bin/sh`
- `mkdir /mnt/folderName`
- you can `chmod -R 777 /mnt/folderName` for good measure

### Adding share
From the right hand menu:

- select jails
- select the jail you just added the folder to
- add a share
- link the folder from the main OS to the newly created folder
- uncheck `create directory`
- click ok

#Nas

### Installation (using OSX)

[Download](http://www.freenas.org/download-freenas-release.html) the latest version.

Ensure the your usb drive only has 1 partition

```
diskutil unmountDisk /dev/disk2
```

Unmount of all volumes on disk1 was successful

```
dd if=FreeNAS-9.3-RELEASE-x64.iso of=/dev/rdisk2 bs=64k
```
### Mounting a FAT USB stick

To find the location of the drive, go to `storage > view disks`. You should be able to see the thumb drive listed as something to `da1`. Make a directory, e.g `/mnt/usb` and then run:

```
mount_msdosfs /dev/da1s1 /mnt/usb
```

To remove the drive.

```
unmount /dev/da1s1
```

### Mounting a NTFS USB drive
Mounting the drive requires you to know the origin to link  to, so **BEFORE** you plug you USB drive in, 

`ls /dev` 

This will give you a list of hard drives already attached, or *mounted* to the NAS.

Now plug your USB drive in.

Load the rtfs module

```
dmesg
kldload fuse
```

Make a directory to house *(mount)* the drive

```
mkdir /mnt/usbDrive
ntfs-3g /dev/ada1s1 /mnt/usbDrive
```

To copy from the file system to the usb file system

```
cp -r -v /mnt/{Tank name}/folder/location /mnt/usbDrive/desiredFolder
```

To copy from the usb to the location file system

```
cp -r -v /mnt/usbDrive/desiredFolder /mnt/{Tank name}/folder/location
```

When done, unmount the drive and remove the then mount point folder ( it is now empty)

```
umount /mnt/usbDrive
rmdir /mnt/usbDrive
```

#### Untested rsync notes
```
rsync -rtvzh source_folder/ destination_folder/
rsync -rtvuh /mnt/tank/media/audio/ /mnt/usbDrive/audio
rsync -ah /mnt/tank/media/audio/ /mnt/usb/audio/
```
### Plex Update Setup

```
jexec {plexJailIdOrName} /bin/sh
```
Get the updater script from [git repo](https://github.com/mstinaff/PMS_Updater/)

```
wget https://raw.githubusercontent.com/mstinaff/PMS_Updater/master/PMS_Updater.sh
```
Exit the jail
```
exit
```
Then run the update command
```
jexec {plexJailIdOrName} /PMS_Updater.sh -a -v -u {username} -p {password}
```
This could be run 

### Plugin setup
- Install PBI
- Add transfer and library folders inside plugin in jail

```
jexec {jailid} mkdir /mnt/transfer /mnt/library
```

- confirm folder permissions
- when adding share

