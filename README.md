# pi-wireless-drive
Options for large wireless backup using rpi


for small files consider a pi git repo:

https://github.com/alhockly/pi-gitserver

-----
MacOS / Time Machine options: (This is just a samba (smb) server with config for TimeMachine on MacOS, it is also accessible on Windows as a network drive)

Samba docs: https://www.samba.org/samba/docs/current/man-html/samba.7.html

requires: pi zero (or pi zero wireless), usb hub, usb-ethernet adapter (or usb wifi), usb HDD
You must you a ext4 file system for this method

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

(if using a spinning disk)

pi
1.  `sudo apt install hdparm`
2. use `blkid` to find the uuid of the disk you are using
3. `sudo hdparm -S 120 /dev/disk/by-uuid/ \< uuid of disk\>`
4. We can make this permanent by adding a stanza to /etc/hdparm.conf:

eg.
>/dev/disk/by-uuid/e613b4f3-7fb8-463a-a65d-42a14148ea65 {
>	spindown_time = 120}

(if you want to make the enclosure) - requires (M2,7 or M3) & M5 bolts
1. print the main body. The orientation should have the mouth of the drive enclosure pointing upwards
2. print the pi cover
3. connect power and usb hub cables to pi
4. decide if you want to drill the mounting holes on your pi wider to accomedate M3 bolts (I did, with a hand drill)
5. screw bolts through the pi cover, the pi itself and into the main body
6. insert the hdd into the enclosure and screw in M5 bolts to stop it from falling out



some articles I used

>[install samba on pi] https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/
>[time machine on remote share] https://www.imore.com/how-use-time-machine-backup-your-mac-windows-shared-folder
>https://wiki.samba.org/index.php/Configure_Samba_to_Work_Better_with_Mac_OS_X
>https://www.thedigitalpictureframe.com/installing-samba-on-your-raspberry-pi-buster-and-optimizing-it-for-macos-computers/


~AFP~ is deprecated
~https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1~


