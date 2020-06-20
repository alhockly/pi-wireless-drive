# pi-wireless-drive
Options for large wireless backup using rpi


for small files consider a pi git repo:

https://github.com/alhockly/pi-gitserver

-----
MacOS / Time Machine options:

Samba + avahi-deamon 

1. create sparse disk image on mac, transfer to online disk
2. mount .dmg on online disk
3. run `sudo tmutil setdestination /Volumes/<dmg name>`
4. open time machine in system prefs, start backup

https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/

https://www.imore.com/how-use-time-machine-backup-your-mac-windows-shared-folder

~or afp~ is deprecated
~https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1~


------
Nas:  
pi + OpenMediaServer
