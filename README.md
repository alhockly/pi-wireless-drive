# pi-wireless-drive
Options for large wireless backup using rpi


for small files consider a pi git repo:

https://github.com/alhockly/pi-gitserver

-----
MacOS / Time Machine options:

Samba + avahi-deamon 

pi

1. `lsblk` to show external devices
2. setup fstab to mount your external disk (exFat) https://askubuntu.com/a/165462 use `sudo mount -av` to mount
e.g `/dev/sda1  /disk exfat defaults,auto,uid=1000,gid=1000,users,rw,nofail 0 1`

3. setup samba and avahi-deamon on pi
4. shutdown pi and connect the disk to mac via usb

Mac

1. create disk image on the external disk (Disk Utility > New Image > Blank Image), set Format to Mac OS Extended (Journaled)
2. connect the disk back into the pi and boot up
3. Finder > Go > Connect to server
4. smb://<ip>  (or use bonjour .local address)
5. double click the .dmg on the remote location to mount
6. run `sudo tmutil setdestination /Volumes/<dmg name>` to enable timemachine to the disk
7. open time machine in system prefs, start backup

https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/

https://www.imore.com/how-use-time-machine-backup-your-mac-windows-shared-folder

~or afp~ is deprecated
~https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1~


------
Nas:  
pi + OpenMediaServer
