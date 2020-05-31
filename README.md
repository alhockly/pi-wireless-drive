# pi-wireless-drive
Options for large wireless backup using rpi


Version control for small files:
https://github.com/alhockly/pi-gitserver


MacOS / Time Machine options:

Samba
https://mudge.name/2019/11/12/using-a-raspberry-pi-for-time-machine/
sudo nano /etc/samba/smb.conf
[backups]
    comment = Backups
    path = /gitRepos
    valid users = pi
    browseable = yes
    available = yes
    writable = yes
    public = yes
    read only = no
    vfs objects = catia fruit streams_xattr
    fruit:time machine = yes
    create mask = 0777
    directory mask = 0777

  


or

~afp~

Deprecated
https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/APFS_Guide/FAQ/FAQ.html#//apple_ref/doc/uid/TP40016999-CH6-DontLinkElementID_1



Nas:  
pi + OpemMediaServer
