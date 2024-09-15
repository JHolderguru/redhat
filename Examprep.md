
#1 - 1
#### Configure your Host Name, IP Address, Gateway and DNS.
#### Host name: station.domain40.example.com
#### /etc/sysconfig/network
#### hostname=abc.com
#### hostname abc.com

#### IP Address:172.24.40.40/24 -

#### Gateway172.24.40.1 -

#### DNS:172.24.40.1 -

```javascript
nmtui
edit connection (GUI enter)

edit ip address to 172.24.40.40/24
edit Gateway to 172.24.40.1

ok
back

activate connection (GUI)
ok
back

set system hostname
ok
back

(check)
(host) hostname
(ip) ifconfig
(Gateway) route -n
(DNS) cat /etc/resolv.conf

```

#62 - 14
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
 # 6 chcon system_u:objectls -lZ /etc/shadow

 # 7 exec /sbin/init

```

#### Add 3 users: harry, natasha, tom.
The requirements: The Additional group of the two users: harry, Natasha is the admin group. The user: tom's login shell should be non-interactive

optional - if users should have password as password

```javascript
 RHEL 9.1 [root@server9 ~]#
 [root@server9 ~] #groupadd admin
 [root@server9 ~]#
 [root@server9 ~]# useradd -G admin harry
 [root@server9 ~]# useradd -G admin natasha
 [root@server9 ~]#
 [root@server9 ~]# useradd -s /sbin/nologin tom [root@server9 ~]#

 #optional
 passwd sarah
 password
 (verify with sudo)...

 ```
 #### Create a catalog under /home named admins. Its respective group is requested to be the admin group. The group users could read and write, while other users are not allowed to access it. The files created by users from the same group should also be the admin group.

 ```javascript

# Create the directory mkdir
1. /home/admins  

# Change the owner of the directory to root
2. sudo chown root: /home/admins  

# Change the group of the directory to admin sudo
3. chgrp admin /home/admins  

# Set the permissions so that the owner and the group can read and write, # but others cannot access it

4. sudo chmod 2770 /home/admins  

# Set the group ID on the directory so that files created within it
# inherit the group of the directory (admin) rather than the group of the user that created the file

5. sudo chmod g+s /home/admins

  ```

#### Configure a task: plan to run echo hello command at 14:23 every day.
  ```javascript
  # crontab -e
  23 14 * * * echo "hello"
  # systemctl restart crond
  # crontab -l

running every minute for a user called harry

# crontab -e -u harry
*/1 * * * * logger "ex200 example"
# systemctl restart crond
# crontab -l

note:
• Minutes  • Hours   • Day of month • Month • Day of week      • Command

   ```

#### Configure a default software repository for your system.One YUM has already provided to configure your system on http://server.domain11.example.com/pub/ x86_64/Server, and can be used normally.

  ```javascript
#dnf repolist
#cd /etc/yum.repos.d
#vi local-rhel9.repo


  [LocalRepo_BaseOs]
  name=local-RHEL
  enabled=1
  gpgcheck=0
  baseurl=http://server.domain11.example.com/pub/ x86_64/Server/BaseOS/

  [LocalRepo_AppStream]
  name=LocalRepo_AppStream
  enabled=1
  gpgcheck=0
  baseurl=http://server.domain11.example.com/pub/ x86_64/Server/AppStream/
  ~  
#dnf clean all
#dnf repolist
  ```

##SELINUX

#### In your system, http serice has some files in .]/var/www/html (do not change or alter files)solve the problem httpd service of your service having some issues, service is not running on port 82.

```javascript
systemctl status

semanage port -l | grep 80
semanage port -a -t http_port_t -p tcp 82
semanage port -l | grep 80

systemctl restart httpd
systemctl enable httpd

curlservera.example:82
(you see the page)
```
#### SELinux must run in force mode.
```javascript
#vim /etc/selinux/config  
    SELINUX=enforcing

#getenforce
SELINUX=enforcing
```

#72
#### Who ever creates the files/directories on archive group owner should be automatically should be the same group owner of archive.

```javascript
#Create a Directory
mkdir archive
#Set the GUID  
chmod g+s archive  

Now any file or folder created in the archive will have the same owner:group permissions as parent folder, in this case same as archive folder
```
#110 - 8
#### Create a backup file named /root/backup.tar.bz2, which contains the contents of /usr/local, bar must use the bzip2 compression.

```javascript
cd /usr/local

tar -jcvf /root/backup.tar.bz2*

Extract:
mkdir /test

tar -jxvf backup.tar.bz2 -C /test

Now any file or folder created in the archive will have the same owner:group permissions as parent folder, in this case same as archive folder
```

#89 - 9
##### Copy /etc/fstab document to /var/TMP directory. According the following requirements to configure the permission of this document.
✑ The owner of this document must be root.
✑ This document belongs to root group.
✑ User mary have read and write permissions for this document.
✑ User alice have read and execute permissions for this document.
✑ Create user named bob, set uid is 1000. Bob have read and write permissions for this document.
✑ All users has read permission for this document in the system.

```javascript
# cp /etc/fstab /var/tmp
# chown root:root /var/tmp/fstab

# useradd mary
# useradd alice
# useradd -u 1000 bob

# setfacl -m u:mary:rw- /var/tmp/fstab
# setfacl -m u:alice:r-x /var/tmp/fstab
# setfacl -m u:bob:rw- /var/tmp/fstab
# chmod o+r-- /var/tmp/fstab

# getfact /var/tmp/fstab (to verify all permissions)
# ls -ltr /var/tmp/fstab (to verify all permissions)

```

#33 - 10
#### Configure NTP service, Synchronize the server time, NTP server: classroom.example.com

```javascript
vi /etc/chrony.conf
  server classroom.example.com iburst
systemctl restart chronyd
systemctl enable chronyd

chronyc sources -v
```

#37 - 11
#### Find out files owned by jack, and copy them to directory /root/findresults

```javascript
find / -user jack -type f -exec cp -rf {} /root/findresults/ \;

ls /root/findresults/
or

vi /opt/find-results.sh

#!/bin/bash
find / -user jack -type f -exec cp -rf {} /root/findresults/ \;

wq!
chmod 755 /opt/find-results
```

#38 - 12
#### Find out all the columns that contains the string seismic within /usr/share/dict/words, then copy all these columns to /root/lines.tx in original order, there is no blank line, all columns must be the accurate copy of the original columns.

```javascript
grep seismic /usr/share/dict/words > /root/lines.txt

or

vi /opt/searchfile.sh
#!/bin/bash
grep seismic /usr/share/dict/words > /root/lines.txt

chmod 755 searchfile.sh

(optional is asked)
cp /opt/searchfile.sh  /usr/local/bin
```

#21 - 13
#### Add user: user1, set uid=2334 -
#### Password: redhat -
#### The user's login shell should be non-interactive..

```javascript
useradd -u 2334 -s /sbin/nologin user1

passwd user1
redhat

```

#20 - 13
#### Add admin group and set gid=600 -

```javascript
sudo -i
groupadd -g 600 admin
cat /etc/group

```

#15/111- 15 -
#### Create a volume group, and set 16M as a extends. And divided a volume group containing 50 extends on volume group lv, make it as ext4 file system, and mounted automatically under /mnt/data.
```javascript
#vgcreate -s 16M vg01 /dev/sda1 /dev/sda2

#lvcreate -l 50 -n lv01 vg01

#mkfs.ext4 /dev/mapper/vg01_lv01

#mkdir /mnt/data

#lsblk -pf (UUID=dde8c40f-fa74-4290-8ff9-252c614e8307)

#echo “UUID=dde8c40f-fa74-4290-8ff9-252c614e8307 /mnt/data ext4 defaults 0 0” >> /etc/fstab

#mount -a

#df -h

```


#### Create a new logical volume according to the following requirements:
#### The logical volume is named database and belongs to the datastore volume group and has a size of 50 extents.
#### Logical volumes in the datastore volume group should have an extent size of 16 MB.
#### Format the new logical volume with a ext3 filesystem.
#### The logical volume should be automatically mounted under /mnt/database at system boot time.

```javascript
#lsblk -pf (to check unused disks and partitions - let's say sda1 and sda2 are empty)
#pvcreate /dev/sda1 /dev/sda2

#vgcreate -s 16M datastore /dev/sda1 /dev/sda2

#lvcreate -l 50 -n database datastore #mkfs.ext3 /dev/datastore/database
#mkdir -p /mnt/database
#lsblk -pf (to see the UUIDs) #echo 'UUID=XXX /mnt/data ext3 defaults 0 0' >> /etc/fstab (use real UUID of "database" volume from previous step instead of XXX)

#systemctl daemon-reload
#mount -a
```

#### Create a 512M partition, make it as ext4 file system, mounted automatically under /mnt/data and which take effect automatically at boot-start.

```javascript
#sudo su
#lsblk -psf (to check for empty disk)
#fdisk /dev/sd[] (format disk in question)
#n (new partition)
#p (for primary)
#Enter (use the first sector by default) #+size 512M (to specify the size)
#Enter
#w (to write the changes)
#lsblk -psf(to verify partition has been created)
#mkfs.ext4 /dev/sd[]1 (to format the partition with ext4 file system)
#mkdir /mnt/data (to create the mount point) #lsblk -psf (to show the UUID for the newly created file system)
#echo 'UUID=XXX /mnt/data ext4 defaults 0 0' >> /etc/fstab #systemctl daemon-reload
#mount -a
```

#### 7-16 Create a 2G swap partition which take effect automatically at boot-start, and it should not affect the original swap partition
```javascript
lsblk (to check partition)
#fdisk /dev/sdb (to sda or sdb depending on the disk)
#n (for new)
#p (primary 1)
#1 (default partition number)
#press enter (first sector)
#+2G (last sector)
#t (to change type and select 82 for swap)
#82 (code for hex)  
#w (save changes)
#partprobe /dev/sbd1 (to update partition table)  
#lsblk(to confirm)
#mkswap /dev/sbd1 (to generate UUID, we need UUID to make it permanent)
#lsblk -f (to see file system, should be swap)
#vi /etc/fstab (to make it permanent) #UUID=xxxxxxxxxxxx swap swap defaults 0 0  
#mount -a
#swapon -s  
free -h #swap value must be increased by 2G
```
#### Change the logical volume capacity named vo from 190M to 300M. and the size of the floating range should set between 280 and 320. (This logical volume has been mounted in advance.)

```javascript
lvdisplay (Check lv)
lvextend -r -L +110M /dev/vg2/
mount -a
```

#### Add an additional swap partition of 754 MB to your system.
The swap partition should automatically mount when your system boots.
Do not remove or otherwise alter any existing swap partitions on your system.
```javascript
fdisk -l
fdisk -cu /dev/vda
p n
e or p select e
default (first): enter
default (last): enter n
default(first): enter
default(first): +754M t (1-5)
l: 82 p
w #reboot
#mkswap /dev/vda5
vim /etc/fstab
/dev/vda5 swap swap defaults 0 0
:wq
mount -a
swapon -a
swapon -s
```

SIMULATION -
Configure autofs to automount the home directories of LDAP users as follows: host.domain11.example.com NFS-exports /home to your system.
This filesystem contains a pre-configured home directory for the user ldapuser11 ldapuser11's home directory is host.domain11.example.com /rhome/ldapuser11 ldapuser11's home directory should be automounted locally beneath /rhome as /rhome/ldapuser11
Home directories must be writable by their users
ldapuser11's password is 'password'.


```javascript
vim /etc/auto.master.d/direct.autofs / -/etc/auto.direct vim /etc/auto.direct ldapuser11 -rw,sync,nstype=nfs4 host.domain11.example.com:/home systemctl restart autofs systemctl enable --now autofs
```

Configure your system so that it is an NTP client of server.domain11.example.com

```javascript
#system-config-date
Note: dialog box will open in that
Check mark Synchronize date and time over network. Remove all the NTP SERVER and click ADD and type server.domain11.example.com
****************And then press ENTER and the press OK***************
```

#### Create a new logical volume according to the following requirements:
The logical volume is named  database and belongs to the datastore volume group and  has a size of 50 extents.
 Logical volumes in the #### datastore volume group should have an extent size of 16 MB.
Format the new logical volume with a ext3 filesystem.
The logical volume should be automatically mounted under /mnt/database at system boot time.

```javascript

vgs    (check if vg is created)
fdisk /dev/vdb
(n - p - (enter) then Last part - +1G - t -l (list) Linux LVM - (30) - w (write and quit))

pvcreate /dev/vdb2
pgs

vgcreate datastore -s 16M /dev/vdb2

vgs

lvcreate -n database -l 50 datastore

lvs

mkfs.ext3 /dev/datastore/database

df -Th
entry into vi /etc/fstab
"/dev/mapper/datastore-database /mnt/database/               ext3       defaults   0 0 "

mkdir /mnt/database

mount /dev/datastore/database /mnt/database/

mount -a

systemctl reload-daemon










```
