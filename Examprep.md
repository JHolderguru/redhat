

#### You are new System Administrator and from now you are going to handle the system and your main task is Network monitoring, Backup and Restore. But you don't know the root password. Change the root password to redhat and login in default Runlevel.

```javascript
 # Restart the System.

 # When the boot-loader menu appears, press any key to interrupt the countdown,
except Enter.

 # - press "e" letter to edit
 # - Remove any console= options from the line.

 # 1 Add init=/bin/bash ...at the end of de line that begins with "Linux" in grub menu(ctrl e)

 # 2 crtl x

 ----bash-5.1#.

 # 3 mount -o remount,rw /

 # 4 ls -lZ /etc/shadow (note this dir system_u:object...)

 # 5 passwd
   redhat
   redhat
 # 6 chcon system_u:object... /etc/shadow

 # 7 exec /sbin/init

```
