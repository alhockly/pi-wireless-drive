# pi-wireless-drive
Options for large wireless backup using rpi


for small files consider a pi git repo:

https://github.com/alhockly/pi-gitserver

-----
MacOS / Time Machine options:

Samba + avahi-deamon

some articles I used

>[install samba on pi] https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/
>[time machine on remote share] https://www.imore.com/how-use-time-machine-backup-your-mac-windows-shared-folder
>https://wiki.samba.org/index.php/Configure_Samba_to_Work_Better_with_Mac_OS_X
>https://www.thedigitalpictureframe.com/installing-samba-on-your-raspberry-pi-buster-and-optimizing-it-for-macos-computers/

you must you a ext4 file system for this method

pi 
 
`sudo apt-get install samba avahi-daemon` say no in Samba config

0. change the pi host name in `sudo nano /etc/hostname` and `sudo nano /etc/hosts` , format disk to ext4 e.g `sudo mkfs.ext4 /dev/<disk parition>`
1. `lsblk` to show connected external devices and `sudo parted -l` to show disk partitions and file systems
2. ~setup fstab to mount your external disk (ext4) https://askubuntu.com/a/165462 use `sudo mount -av` to mount
e.g `/dev/sda1  /media/disk ext4 defaults,auto,uid=1000,gid=1000,users,rw,nofail 0 0`~
2. add `sudo mount /dev/sda1 /media/disk` to crontab. (`crontab -e`) 
3. configure samba `sudo nano /etc/samba/smb.conf` , use the one below as a template. You should only need to change the lines under the `[Time Capsule]` definition
	https://github.com/alhockly/pi-wireless-drive/blob/master/smb.conf
4. restart samba `sudo smbcontrol smbd reload-config`
5. add user to samba `sudo smbpasswd -a <username>`
6. shutdown pi and connect the disk to mac via usb


Mac
1. check network in finder and you should see the samba share by its hostname. if not go to Finder > Go > Connect to server then enter smb:// \<ip address\>
2. open time machine in system prefs, select the network disk
3. start backup


pi
1. (if using a spinning disk) `sudo apt install hdparm`
2. use `blkid` to find the uuid of the disk you are using
3. `sudo hdparm -S 120 /dev/disk/by-uuid/ \< uuid of disk\>`
4. We can make this permanent by adding a stanza to /etc/hdparm.conf:

eg.

>/dev/disk/by-uuid/e613b4f3-7fb8-463a-a65d-42a14148ea65 {
>	spindown_time = 120}


~AFP~ is deprecated
~https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1~


