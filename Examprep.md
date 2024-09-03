

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

##Directories
#### Create a catalog under /home named admins. Its respective group is requested to be the admin group. The group users could read and write, while other users are not allowed to access it. The files created by users from the same group should also be the admin group.

```javascript
mkdir -p /sha
```
