# pi-wireless-drive
Options for large wireless backup using rpi


for small files consider a pi git repo:

https://github.com/alhockly/pi-gitserver

-----
MacOS / Time Machine options:

Samba + avahi-deamon 
[install samba on pi] https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/
[time machine on remote share] https://www.imore.com/how-use-time-machine-backup-your-mac-windows-shared-folder

https://www.thedigitalpictureframe.com/installing-samba-on-your-raspberry-pi-buster-and-optimizing-it-for-macos-computers/

pi
`sudo apt-get install exfat-fuse exfat-utils samba avahi-daemon` say no Samba config?
0. change the pi host name `sudo nano /etc/hostname`
1. `lsblk` to show connected external devices and `sudo parted -l` to show disk partitions and file systems
2. setup fstab to mount your external disk (exFat) https://askubuntu.com/a/165462 use `sudo mount -av` to mount
e.g `/dev/sda1  /home/pi/disk exfat defaults,auto,uid=1000,gid=1000,users,rw,nofail 0 0`
mount in home dir??
3. configure samba `sudo nano /etc/samba/smb.conf` and restart samba `sudo smbcontrol smbd reload-config`
https://wiki.samba.org/index.php/Configure_Samba_to_Work_Better_with_Mac_OS_X
4. shutdown pi and connect the disk to mac via usb


Mac

1. create disk image on the external disk (Disk Utility > New Image > Blank Image), set Format to Mac OS Extended (Journaled)
2. connect the disk back into the pi and boot up
3. Finder > Go > Connect to server
4. smb://<ip>  (or use bonjour .local address)
5. double click the .dmg on the remote location to mount
6. run `sudo tmutil setdestination /Volumes/<dmg name>` to enable timemachine to the disk
7. open time machine in system prefs, start backup



pi
1. (if using a disk) `sudo apt install hdparm`
`sudo hdparm -S 120 /dev/disk/by-uuid/e613b4f3-7fb8-463a-a65d-42a14148ea65`
We can make this permanent by adding the following stanza to /etc/hdparm.conf:

/dev/disk/by-uuid/e613b4f3-7fb8-463a-a65d-42a14148ea65 {
	spindown_time = 120
}


~or afp~ is deprecated
~https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1~


------
Nas:  
pi + OpenMediaServer
