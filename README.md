## Essential Commands: 

### Connecting to Local and Remote Consoles
```
ssh-keygen
ssh-copy-id
```

______________________________________________
### Creating, Deleting, Copying, and Moving Files and Directories
```
ls
cd directory/
cd ~
cd ..
ls -l
ls -lh
ls -la
cp file1 file2
mv file1 directory/
mv file1 file1newname
mkdir folder
rmdir folder/
rmdir -r folder/
rmdir -rf folder/
touch filename
```
______________________________________________
### Searching for Files
```

find (realtime , powerful, slow)
locate (search indexsnapshot , fast)
which ( search $PATH only and gives you the full path , only executable files , fast)
find directory | grep name
```
______________________________________________
### Manipulating Searched Files

```
find .
find -type d(directory)
find -type f(file)
find -type f -size +1M
find -type f -size +1M -size -100M
find -type f -size +100M
find -type f -size +100M -delete
find . -type f -size +1M -exex ls -lh  {} \;
find . -type f -size +1M -exex rm -rf  {} \;
```
______________________________________________
### Evaluating and Comparing Filesystems

```
ext4 (stable, kernel support, usually default) Best choice !
zfs (snapshots, span drives, resizeable, proprietary-ish)
btrfs ( linux native (next gen) file system, redhat in 2017 abandoned , SUSE supports)
XFS ( good with large files, separate tools, getting  old)
RieserFS(was the next big thing, Dev went to prison, no body used it much)
```
______________________________________________
### Comparing File Content
```

diff file1.txt file2.txt
diff -c file1.txt file2.txt
md5sum *
```
______________________________________________
### Using Input-Output Redirection

```
watch -d -t -n 1 cat results.txt
cat file.txt > results.txt
cat file.txt >> results.txt
cat someError.txt 2> results.txt
cat someError.txt 2> results.txt 2>&1
wc (word count) -w file.txt
wc -w <file.txt
cat file.txt | wc -w
```
______________________________________________
### Analyzing Text With Regular Expressions

Globbing :
```

 *.*  
 *.doc 
 file?.doc
```

Regex :
```

 . = any character
 * = zero or more of previous character
 ^ = beginnig of line
 $ = end of line 
 \s = whitespace (space, tab, newline)
 | = OR
 [a-z,0-9] = or with range support
 .* = construct meaning zero or more of any character
```
Examples :
```
grep "chicken" file.txt
grep "[Cc]hicken" file.txt
grep "\scorn" file.txt
grep "^chicken" file.txt
grep "^[Cc]hicken" file.txt
grep "corn$" file.txt
grep "^chicken.*corn$" file.txt
```
______________________________________________
### Using tar to Create and Extract Files and Folders
```

tar -c -v- f archive.tar directory/
tar -xvf archive.tar

flags :
-c : create
-x : extract
-v : verbose
-f : filename (Has to be at the end like -cvf)
```

______________________________________________
### Using Compression Schemes with Tar
```
-z : tar -z -c -v- f archive.tar.gz directory/  

-j : tar -j -c -v- f archive.tar.bz2 directory/

-J : tar -J -c -v- f archive.tar.xz directory/
```

______________________________________________
### Using Other Compression Tools
```
gzip document.pdf => document.pdf.gz
gunzip document.pdf.gz 
gzip can't archive a directory , It will compress every individual file in that directory separately!

zip file.zip document.pdf 
zip -r allfiles.zip directory/
unzip allfiles.zip
```

______________________________________________
### Creating and Managing Hard and Soft Links
```

ln file.txt hardfile.txt
ln -s file.txt softfile.txt
ls -li
```
______________________________________________
### Handling inodes and Multi-Linked Files
```

stat file.txt
find . -samefile file.txt
```
______________________________________________
### Listing, Setting, and Changing Standard File Permissions
```

type    | user | group | other || owner | group affiliation | name
- or d    r w x     r w x     r w x      user             group               name


 sudo chown root filename.txt
 sudo chgrp root filename.txt

 sudo chown ehsan.ehsan filename.txt

sudo chmod ugo-x filename.txt
sudo chmod u-x,g-x,o-x foldername/

if a directory is executable it means you can cd to that directory
```
______________________________________________
### Octal Notation of Standard File Permissions
```
r = 4   w = 2  z = 1
rw-  = 6 
-wx  = 3
r-x  = 5
--x  = 1
-w-  = 2
r-- = 4

chmod 666 filename.txt 
-rw-rw-rw-
chmod 777 filename.txt 
-rwxrwxrwx
chmod 751 filename.txt 
-rwxr-x--x
```
______________________________________________
### SUID, SGID, and Sticky Bits
```
SUID = 
File : executable runs with owner permissions
Folder : Nothing

GUID = 
File : executable runs with group permissions
Folder : new files created with matching group membership

Sticky bit =
File : Nothing
Folder : files can't be deleted by non-owner 

sudo chmod o+t shareddirectory/
```
______________________________________________
### Reading and Using System Documentation

```
cd /usr/share/doc

ls --help

man ls

whatis passwd

man 5 passwd

man -k 
```
______________________________________________ 
### Managing Access to the Root Account
```

su = become root with password
sudo command = run the command with root privilege
sudo su = become root without password
sudo visudo = Edit sudoers file and allow a user to execute any command without asking password
	sally ALL=(ALL:ALL) NOPASSWD:ALL
```
______________________________________________ 
## Operation of Running Systems: 

### Booting, Rebooting, and Shutting Down a System Safely

```
programs:
    
	halt command
	reboot command
	shutdown command
```
______________________________________________ 
### Booting or Changing System into Different Operating Modes
```

    systemd             sysV

poweroff.target    =>  runlevel 0
rescue.target      =>  runlevel 1
multi-user.target  =>  runlevel 3
graphical.target   =>  runlevel 5
reboot.target      =>  runlevel 6

as a root user :
	
systemctl get-default

systemctl set-default multi-user.target

systemctl isolate reboot.target
```

______________________________________________ 
### Installing, Configuring, and Troubleshooting Bootloaders
 
```
sudo nano /etc/default/grub

sudo update-grub

```
______________________________________________ 
### Diagnosing and Managing Processes

```
top (see what process using your CPU more than others)

ps -ef | grep dd (dd stands for name of that process)

kill 1750 (1750 stands for process id [PID])

```
______________________________________________
### Locating and Analyzing System Log Files

```
as root user :

cd /var/log

grep ssh *

tail syslog
tail -f syslog (it's realtime)
tail -n (will show you the last 20 lines)
```

______________________________________________
### Scheduling Tasks with User Crontab

```
crontab -e (to edit)
```

______________________________________________
### Scheduling System Tasks to Run at a Set Date and Time

```
/etc/crontab
/etc/cron.d/*
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
```

as a root user :

```
nano /etc/crontab

cd /etc/cron.d/
 nano popularity-contest
 
```
______________________________________________ 
### Scheduling One-Time Tasks

```
at now + 1 minute (Schedules future one-time event)
 at> echo "test" > results.txt
 at> echo "still testing" > results.txt
 
when you're done, press Ctrl + d 

echo "another way" > way2.txt | at now + 1 minute

atq (shows the current queued items)

atrm 4 (Removes queued job 4)
```

______________________________________________ 
### Verifying Completion of Scheduled Tasks

```
1- looking to /var/log/syslog :
 
grep CRON /var/log/syslog

2- Email notification

By default, crontab jobs email the STDOUT and STDERR to the user

But :

crontab -e
 
 35 6 * * * echo "Backups Completed" > /dev/null
 
 "It's going to send an email to you ONLY if the task failed"
```
 
______________________________________________ 
### Updating Software to Provide Required Functionality and Security

```
apt install
apt remove
apt update
apt upgrade
apt search
apt info
```
______________________________________________
### Identifying Which Component of a Linux Distribution a File Belongs

```
If the package was installed before :

	sudo rm /usr/bin/zipcloak

	dpkg -S /usr/bin/zipcloak 

		zip

	sudo apt purge zip

	sudo apt install zip

If the package was Not installed before :

	apt-file search /usr/bin/zipcloak

```
______________________________________________
### Verifying the Integrity of Filesystems

```
as a root user:

mount

cd /mnt/sda5

making some corruption : 

	dd if=/dev/zero of=/dev/sda5 bs=1k count=3

cd
unmount /mnt/sda5
fsck /dev/sda5
yes
yes
yes
yes
mount /dev/sda5 /mnt/sda5

Another way:

restart 
press shift in grub menu
at the end of line which starts with linux add this : fsck.mode=force
press ctrl + X

```
______________________________________________
### Testing Memory

```
as a root user :

free -h 

memtester 8G 1

memtester 6200M 1 (should be free)

Another way:

restart 
press shift in grub menu
select memory test 86+ (it will test all of the capacity of your RAM)

```
______________________________________________
### Using Scripting to Automate System Maintenance Tasks

```
#!/bin/bash

for var in 1 2 3 4  OR [1..10] OR $(a linux command)
do
	echo "loop $var"
	echo "loop complete"
done

```
______________________________________________
### Listing and Identifying the AppArmor File and Process Contexts

```
ps -eZ

aa-status
---------------------

cd /etc/apparmor.d/

nano /usr.sbin.cupsd
```

______________________________________________
## User and Group Management: 

### Creating, Deleting, and Modifying Local Users

```
adduser testuser
useradd -h (need flags) 
usermod -a for append -G to other groups
userdel -rf
```
______________________________________________
### Creating, Deleting, and Modifying Local Groups

```
groupadd -h 
groupmod
groupdel
```

______________________________________________
### Managing System-wide Environment Profiles
```

All users :
	/etc/environment
	/etc/bash.bashrc
	/etc/profile
	
Individual :
	/home/user/.bashrc
	/home/user/.profile

```
______________________________________________
### Managing the User Template
```

/etc/skel

its only for new users 
```
______________________________________________
### Changing User Limits With Ulimit

```
ulimit

	hard limit (-H) : is the kernel-enforced maximum a user can set
	soft limit (-S) : is the current limit which can be adjusted by user until hard limit is met

ulimit -Ha (current hard limits)
ulimit -Sa (current soft limits)

ulimit -Sn 4000 (soft limit for open files)
```
______________________________________________
### Configuring System-wide User Resource Limits
```

nano /etc/security/limits.conf
```
______________________________________________
### Configuring PAM

```
/etc/pam.d/

pam-auth-update

```
______________________________________________
## Networking: 

### Configuring with the GUI

you can use Network manager for it ! so simple :D
______________________________________________
### Configuring with Config Files
```

as a root user :

nano /etc/network/interfaces
- - - - - - - - - - - - - -

# Configure primary iterface with DHCP
auto eth0
iface eth0 inet dhcp

# The primary network interface statically
auto eth0
iface eth0 inet static
		address 10.10.10.20
		netmask 255.255.255.0
		network 10.10.10.0
		broadcast 10.10.10.255
		dns-nameservers 10.10.10.1
		dns-search cbt
		
 save and exit
- - - - - - - - - - - - - -

service networking restart

ifconfig
```
______________________________________________
### Managing Services with Systemctl
```

as a root service:

systemctl                                       name of service
		  enabled: will start on boot           
		  disabled: will not start on boot

		  start: start service
		  stop: stop service
		  restart: restart (or start) service

		  status: show info about service
```

______________________________________________
### Implementing Packet Filtering with iptables
```

as a root user:

ping google.com

iptables -L

iptables -A OUTPUT -d 172.217.18.142 -j DROP

iptables -L

	DROP all -- anywhere		google.com

ping google.command
		
	Operation not permitted

iptables -F

iptables -L

ping google.com
```
______________________________________________
### Setting Up a Static Route
```

as a root user:

ifconfig

ping 192.168.1.1

route

ip route add 192.168.1.0/24 via 10.10.10.20 dev eth0

route

192.168.1.1
```
______________________________________________
### Synchronizing Time with NTP
```

as a root user:

date

ntpdate

ntpdate 10.10.10.1

apt install ntp

nano /etc/ntp.conf

- - - - - - - - - - - - - -
#pool 0.ubunto.pool.ntp.org iburst
#pool 1.ubunto.pool.ntp.org iburst
#pool 2.ubunto.pool.ntp.org iburst
#pool 3.ubunto.pool.ntp.org iburst

END OF FILE :

server 10.10.10.1 
		
 save and exit
- - - - - - - - - - - - - -

systemctl restart ntp

if your server has internet you can use this:

ntp server
```
______________________________________________
## Services: 

### Configuring a Caching DNS Server

```
as a root user :

cd /etc/bind

nano named.conf.options

- - - - - - - - - - - - - -
acl clinets {
	10.10.10.0/24
	localhost;
	};

options { 		
 		recursion yes;
		allow-query {clients; };
		...
		}
 save and exit
- - - - - - - - - - - - - -

systemctl restart bind9

dig @localhost google.com

```
______________________________________________
### Maintaining a DNS Zone

```
as a root user :

cd /etc/bind

named.conf.local 
```

______________________________________________
### Configuring Email Aliases

```
tail -f /var/mail/username

mail -s "test to bob" bob

sudo nano /etc/aliases

- - - - - - - - - - - - - -
turtle: bob
fish: root

 save and exit
- - - - - - - - - - - - - -
sudo newaliases

mail -s "test to fish" fish
mail -s "test to fish" turtle

```
______________________________________________
### Configuring an SSH Server

```

nano /etc/ssh/sshd_config

- - - - - - - - - - - - - -
you can change the port number to listen on a diffrent port

OR

#Authentication
PermitRootLogin yes => no  OR prohibit-password

 save and exit
- - - - - - - - - - - - - -
systemctl restart ssh


```

______________________________________________
### Configuring HTTP Proxy Server with Squid

```
sudo apt install squid

nano /etc/suid/squid.conf

- - - - - - - - - - - - - -
TAG: cache_mem
	...
	cache_mem 256 MB
	...
	maximum_object_size_in_memory 2048 KB

 save and exit
- - - - - - - - - - - - - -

sudo systemctl restart squid.service

add localhost 3128 to proxy setting with firefox
```
______________________________________________
### Restricting HTTP Proxy Access

```
ping google.com +> Network is unreachable

Manual proxy configuration on firefox > http proxy: server1 , port: 3128, check use this proxy server for all protocols

web browser > google.com  access denied!

ssh root@server1

nano /etc/squid/squid.conf

- - - - - - - - - - - - - -
under acl section:
	
acl bob src 10.10.10.10 	

#TAG http_access
	...
	#http_access allow localhost
	
	http_access allow bob
	
	#And finally deny all other access to this proxy
	http_access deny all 

 save and exit
- - - - - - - - - - - - - -

sudo systemctl restart squid.service

web browser > google.com  200!
```
______________________________________________
### Configuring IMAP & IMAPS Services with Dovecot

```
sudo apt install dovcot-imapd

netstat -tuna | grep LISTEN

configure thunder bird manually 

cd /etc/dovecot/conf.d/

ls

sudo nano 10-ssl.conf

- - - - - - - - - - - - - -
# SSL/TLS support: yes, no, required.

ssl = yes
ssl_cert=</etc/ssl/certs/ssl-cert-snakeoil.pem
ssl_ke=</etc/ssl/private/ssl-cert-snakeoil.key


 save and exit
- - - - - - - - - - - - - -
sudo systemctl restart dovecot

configure thunder bird manually 
```

______________________________________________
### Specifying Service Behavior at Various Runlevels (Targets)

```
cd /lib/systemd/system

ls 

cd /etc/systemd/system

ls

systemctl get-default > graphical.target

systemctl list-dependencies graphical.target > tree

cd graphical.target.wants

sudo systemctl enable ssh
```

______________________________________________
### Setting up Apache Web Server
```

sudo apt install apache2 php

cd /var/www/html

sudo mv index.html index.html.bak

sudo nano index.html

- - - - - - - - - - - - - -

!!! TEST !!!

 save and exit
- - - - - - - - - - - - - -

Browse localhost
```
______________________________________________
### Configuring Virtual Hosts
```

cd /etc/apache2/sites-available

ls

cp 000-default.conf fishlips.conf

nano fishlips.conf

- - - - - - - - - - - - - - 

ServerName fishlips

DocumentRoot /var/www/fishlips

 save and exit
- - - - - - - - - - - - - - 

a2ensite fishlips

service apache2 reload

cd /var/www/fishlips

nano /etc/hosts

- - - - - - - - - - - - - - 

10.10.10.10 fishlips

 save and exit
- - - - - - - - - - - - - - 

ping fishlips

browse http://fishlips

```

______________________________________________
### Restricting Access to a Web Server Based on Network Address
```

ifconfig

cd /etc/apache2/sites-available

ls

cp 000-default.conf 

nano 000-default.conf

- - - - - - - - - - - - - - 

DocumentRoot /var/www/html

<Location />
	Require ip 10.10.10.10
</Location>

 save and exit
- - - - - - - - - - - - - - 

systemctl reload apache2.service

browse localhost > 403 Forbidden

browse 10.10.10.10 > 200 Success 

```

______________________________________________
### Installing the MariaDB Server
```

sudo apt install mariadb-server mariadb-client > E: Unable o locate package

sudo apt install mysql-server mysql-client

mysql -u root -p 

Enter password:

mysql> show databases
```

______________________________________________
### Configuring Docker
```

docker run -d -p 8080:80 -v /root/config:/config linuxserver/nginx

docker ps

ls

cd config/
ls
cd www/
ls
rm index.html

nano index.html

- - - - - - - - - - - - - - 

It worked !

 save and exit
- - - - - - - - - - - - - - 

browse localhost:8080
```

______________________________________________
### Managing KVM Virtual Machines with GUI Tools
```

sudo apt install qemu-kvm libvirt-bin virt-manager

sudo virt-manager

+ > Local install media 
```
______________________________________________
### Managing KVM Virtual Machines with CLI Tools
```

as a root user :

virsh list

virsh suspend generic(name)

virsh resume generic(name)

virt-install --name test --ram 512 --vcpu=1 --cdrom=/home/bob/dsl.iso --network=default /
	--disk path=/home/bob/testdisk/qcow,size=200,bus=ide,format=qcow2
```

______________________________________________
## Storage: 

### Managing Physical Partitions with Fdisk

```
ls /dev | grep sd

sudo fdisk /dev/sdb

m

p

n

>p
>1
>2048
>20971519

p

m

t

L

83

m

w

ls /dev | grep sd
```

_____________________________________________
### Creating Filesystems on Partitions
```

ls /dev | grep sd

ls -l /sbin/mk*

mkfs.ext4 /dev/sdb1
```

_________________________________________
### Handling Swap Partitions and Files
```

swapon --show

cd /

dd if=/dev/zero of=/swapspace bs=1M count=500

mkswap /swapspace

swapon /sawpspace

chmod 0600 /swapspace

swapon --show

nano /etc/fstab

- - - - - - - - - - - - - - 
...

/swapspace 		none	swap	sw	0	0

 save and exit
- - - - - - - - - - - - - -

reboot

swapon --show
```
______________________________________________
### Automatically Mount Filesystems with fstab
```

cd /mnt/

ls

showmount -e server1

nano /etc/fstab

- - - - - - - - - - - - - - 
...

/dev/sdb1					/mnt/disk		auto	defaults	0	0
server1:/opt/share/			/mnt/share		nfs		defaults	0	0

 save and exit
- - - - - - - - - - - - - -

mount -a

mount
```
______________________________________________
### Creating an Encrypted Storage Volume
```

1- create encrypted partition

2- decrypt (open) partition
3- format/use partition
4- re-encrypt (close) partition

as a root user: 

apt install cryptsetup

ls /dev | grep sd

cryptsetup luksFormat /dev/sdb1

YES

Enter passphrase:

cryptsetup luksOpen /dev/sdb1 encrypted

Enter passphrase for /dev/sdb1:

ls /dev/mapper

mkfs.ext4 /dev/mapper/encrypted

mkdir /mnt/disk

mount /dev/mapper/encrypted /mnt/disk

cd /mnt/disk

touch testfile.txt

cd

umount /mnt/disk

cryptsetup lukeClose encrypted

ls /dev/mapper

cryptsetup luksOpen /dev/sdb1 myfiles

ls /dev/mapper

mount /dev/mapper/myfiles /mnt/disk

ls /mnt/disk
```
______________________________________________
### Managing LVM Storage
```

ls /dev | grep sd

pvdisplay

pvcreate /dev/sdb

pvdisplay

pvcreate /dev/sdc

pvcreate /dev/sdd

vgdisplay

vgcreate pool /dev/sdb /dev/sdc /dev/sdd

vgdisplay

lvdisplay

lvcreate -L 15G -n chunk pool 

lvdisplay

ls /dev/mapper 

mkfs.ext4 /dev/mapper/pool-chunk
```


______________________________________________
### Managing RAID Devices with mdadm
```

ls /dev | grep sd

mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd

cat /proc/mdstat

ls /dev | grep md

mksf.ext4 /dev/md0

mkdir /mnt/raid

mount /dev/md0 /mnt/raid

ls /mnt/raid

df -h

mdadm --detail --scan

mdadm --detail --scan . /etc/mdadm/mdadm.conf

```

______________________________________________
### Mounting Filesystems on Demand with AutoFS

```
as a root user :

ls /  (looking for nfs )

ls /nfs

nano /etc/auto.master

- - - - - - - - - - - - - - 
...

#/misc  /etc/auto.misc

/nfs   /etc/auto.nfs

 save and exit
- - - - - - - - - - - - - - 

nano /etc/auto.nfs

- - - - - - - - - - - - - - 
server server1:/nfs

 save and exit
- - - - - - - - - - - - - - 

service autofs restart

ls /nfs/server

ls /nfs

mount

```

______________________________________________
### Managing Attributes with chattr and lsattr

```
as a root user:

ls -l

rm important.conf > operation not permitted.

lsattr

chattr -i important.conf

rm important.conf

chattr +a file.txt

lsattr

echo "one more line" >> file.txt

echo "replace with this" > file.txt   operation not permitted.
```

______________________________________________
### Managing Quotas on Linux
```

as a root user:

nano /etc/fstab

- - - - - - - - - - - - - - 
...

/dev/sdb1					/mnt/disk		auto	defaults,usrqouta	0	0


 save and exit
- - - - - - - - - - - - - -

umount /mnt/disk

mount /mnt/disk

mount

qoutacheck -au

qoutaon -a

edqouta bob 

- - - - - - - - - - - - - - 
Filesystem            blocks       soft        hard         inodes        soft         hard                    

/dev/sdb1		0	   500         1000           0             0            0


 save and exit
```

- - - - - - - - - - - - - -

