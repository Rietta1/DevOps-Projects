## DEVOPS-TOOLING-WEBSITE-SOLUTION

### NFS Server

Network File System (NFS) is a protocol that allows a computer to access files over a network as if they were stored locally. It is typically used for sharing files between Linux and Unix systems, but can also be used with other operating systems. NFS allows for file sharing with high performance, transparent file access, and file locking capabilities. It uses Remote Procedure Calls (RPC) to communicate between the client and server.


 NFS is a tremendously helpful technology, although it has traditionally had numerous drawbacks, the most of which have been resolved in protocol version 4. The current version of NFS is more difficult to configure when you wish to utilize basic security features like authentication and encryption because it relies on Kerberos for them. And, in the absence of them, the NFS protocol must be limited to a trusted local network since data travels across the network unencrypted (a sniffer can capture it) and access permissions are provided depending on the client's IP address (which can be spoofed).

As part of a DevOps team, we will create a tooling website solution that allows easy access to DevOps tools within the corporate infrastructure.




![2](https://user-images.githubusercontent.com/101978292/215043712-0b66ff48-d898-4fc2-8b7f-64bb68ba1052.jpg)


### Prerequites
1. Provision 4 Red Hat Enterprise Linux 9. One will be the NFS server and the other as the Web servers.
2. Provision 1 Ubuntu 22.04 for the the databaes server.




![1a](https://user-images.githubusercontent.com/101978292/215045104-1c75d649-4c51-4399-81de-245e8120f28c.jpg)


### Step 1 - Prepare NFS server
1. Create 3 volumes of 10gb each and attach all three volumes one by one to your NFS Server EC2 instance

2. To view all logical volumes, run the command 

```
lsblk
```

The 3 newly created block devices are names **xvdf**, **xvdh**, **xvdg** respectively.
3. Use gdisk utility to create a single partition on each of the 3 disks 
```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```
4. A prompt pops up, type `n` to create new partition, enter no of partition(1), hex code is 8300, `p` to view partition and `w` to save newly created partition.
5. Repeat this process for the other remaining block devices.
6. Type `lsblk` to view newly created partition.
7. Install lvm2 package by typing: 
```
sudo yum install lvm2
``` 
then run 
```
sudo lvmdiskscan
```
 command to check for available partitions.


![n1](https://user-images.githubusercontent.com/101978292/215037731-2f414e18-70aa-473e-8fc0-36b0d57f97d7.jpg)

8. Create physical volume to be used by lvm by using the pvcreate command:
```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```


9. To check if the PV have been created successfully, run: 
```
sudo pvs
```
![n2](https://user-images.githubusercontent.com/101978292/215041997-9ba44095-e046-48db-b758-08c8198031cd.jpg)

10. Next, Create the volume group and name it webdata-vg: 

```
sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```

11. View newly created volume group type: 
```
sudo vgs
```
12. Create 3 logical volumes using lvcreate utility. Name them: **lv-apps** for storing data for the website, **lv-logs** for storing data for logs and **lv-opt** for Jenkins Jenkins server in project 8.

```
sudo lvcreate -n lv-apps -L 9G webdata-vg
sudo lvcreate -n lv-logs -L 9G webdata-vg
sudo lvcreate -n lv-opt -L 9G webdata-vg
```

13. Verify Logical Volume has been created successfully by running:
 ```
sudo lvs
```
![n3a](https://user-images.githubusercontent.com/101978292/215041385-6465717a-478c-4b15-a85a-7392fd6f13c6.jpg)


14. Next, format the logical volumes with ext4 filesystem:

```
sudo mkfs -t xfs /dev/webdata-vg/lv-apps
sudo mkfs -t xfs /dev/webdata-vg/lv-logs
sudo mkfs -t xfs /dev/webdata-vg/lv-opt
```

![n4](https://user-images.githubusercontent.com/101978292/215037895-edbbd1b7-69e4-44be-9305-bfe75d15e3ab.jpg)


15. Next, create mount points for the logical volumes. Create **/mnt/apps** the following directory to store website files: 
```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```
16. Mount to **/dev/webdata-vg/lv-apps** **/dev/webdata-vg/lv-apps** and **/dev/webdata-vg/lv-opt** respectievly : 

```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
sudo mount /dev/webdata-vg/lv-opt /mnt/opt
```

17. Install NFS server, configure it to start on reboot and make sure it is up and running

```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```


![n5](https://user-images.githubusercontent.com/101978292/215037948-d71556b0-5222-4a60-abfb-db36dd557e3b.jpg)

18. Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.
19. Set up permission that will allow our Web servers to read, write and execute files on NFS:
```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service
```

![n6a](https://user-images.githubusercontent.com/101978292/215041756-71a549fd-6acb-49a7-b709-f5a852393a90.jpg)

20. In your choosen text editor, configure access to NFS for clients within the same subnet (my Subnet CIDR – 172.31.80.0/20 ):

```
sudo vi /etc/exports

/mnt/apps 172.31.48.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.48.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.48.0/20(rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

sudo exportfs -arv
```

21. Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)
```
rpcinfo -p | grep nfs
```

![n6](https://user-images.githubusercontent.com/101978292/215038101-3627d240-734d-4590-a8c0-cfae1fc50eba.jpg)


### STEP 2 — CONFIGURE THE DATABASE SERVER
1. Install and configure a MySQL DBMS to work with remote Web Server
2. SSH in to the provisioned DB server and run an update on the server: 
```
sudo apt update
```

3. Install mysql-server: 

```
sudo apt install mysql-server -y
```

4. Create a database and name it **tooling**: 

```
sudo mysql

create database tooling;
```

5. Create a database user and name it **webaccess** and grant permission to **webaccess** user on tooling database to do anything only 
from the webservers subnet cidr:

```
create user 'webaccess'@'172.31.48.0/20/' identified by 'password';

grant all on tooling.* to 'webaccess'@'172.31.48.0/20'with grant option;

flush privileges;
```

6. To show database run: 

```
show databases;
```

```
use tooling;
```

![d1](https://user-images.githubusercontent.com/101978292/215042305-0fab00cf-3bca-45e9-95b2-a66222d9931c.jpg)


### Step 3 — Prepare the Web Servers

1. Update your server

```
sudo yum update -y
```

2. Install NFS client on the webserver1: 

```
sudo yum install nfs-utils nfs4-acl-tools -y
```
3. Mount **/var/www/** and target the NFS server’s export for apps 
(Use the private IP of the NFS server)

```
sudo mkdir /var/www

sudo mount -t nfs -o rw,nosuid 172.31.63.145:/mnt/apps /var/www

```



4. Verify that NFS was mounted successfully by running 

```
df -h
```

![w1](https://user-images.githubusercontent.com/101978292/215032356-02e6afde-603a-45d5-9293-07a4c6d22a93.jpg)

 Make sure that the changes will persist on Web Server after reboot:

 ```
sudo vi /etc/fstab
```

5. Add the following line in the configuration file: 

```
172.31.63.145:/mnt/apps /var/www nfs defaults 0 0

```

6. Install Apache httpd

```
sudo yum install httpd -y

sudo systemctl start httpd

sudo systemctl status httpd

```


7. Install Remi’s repository, Apache and PHP:
(php installation is always changing, use this link for new process:
https://www.linuxshelltips.com/install-php-8-rhel/ )

```
sudo yum update -y

sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

 sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module list php

sudo dnf module reset php

sudo dnf module enable php:remi-8.1 -y 

sudo dnf install php

php -v

sudo dnf install php-{fpm,common,mysqlnd,cli}

```

8. Repeat steps 1-5 for the other 2 webservers

9. Verify that Apache files and directories are available on the Web Server in **/var/www** and also on the NFS server in **/mnt/apps.**

```
ls /var/www

ls /mnt/apps

```


![w4](https://user-images.githubusercontent.com/101978292/215035850-178d089c-1aad-429c-8f11-9294a029f5ec.jpg)


10. Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №4 and №5 to make sure the mount point will persist after reboot:


```
sudo mount -t nfs -o rw,nosuid 172.31.63.145:/mnt/logs /var/log/httpd

df -h

sudo vi /etc/fstab

172.31.63.145:/mnt/logs /var/log/httpd nfs defaults 0 0

```

11. Install mysql client so as to be able to access our website can access our database server remotely.

```
sudo yum install mysql-server -y

```



12. Fork the tooling source code from **Darey.io** Github Account to your Github account. 

13. Begin by installing git on the webserver:
 ```
sudo yum install git -y
```

14. Initialize Git: 
```
git init
```

15. Then run: `git clone https://github.com/darey-io/tooling.git`


![w2a](https://user-images.githubusercontent.com/101978292/215032622-9e7d7f30-bf09-4afb-b2a3-26026e3c07cb.jpg)

16. Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to **/var/www/html**

```
ls /var/www/html

cd tooling
```
17. Copy everything of the html to **/var/www/html**

```
sudo cp -R html/. /var/www/html

```

**NOTE: That apache webserver might not be able to start because of the security presently active on our server, You can either disable the SELinux security with
this command and turned the **selinux enforcing** to **selinux=`disabled`** and restart the system after that:*

```
sudo setenforce 0

sudo vi /etc/selinux/config
```

18. Change the ownership of **html** and **log** folder to Apache and also change the permission of the **html** and **log** folder to Apache


```
sudo chown -R apache:apache /var/www/html

sudo chown -R apache:apache /var/log/httpd 

sudo chmod -R 755 /var/log/httpd 

sudo chmod -R 755 /var/www/html

```


19. On the webserver, ensure port 80 in open to all traffic in the security groups.

20. Update the website’s configuration to connect to the database: 

```
sudo vi /var/www/html/functions.php
```

![w5](https://user-images.githubusercontent.com/101978292/215035954-6c242789-fd0a-4dd8-bc7c-523e8b27699a.jpg)


21. Apply tooling-db.sql script to your database using this command 
`mysqli_connect ('172.31.94.254', 'webaccess', 'password', 'tooling')`

22. Go to the databse server and update the binding

 addresses to 0.0.0.0: 

```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```


23. Login to your databaseserver mysql through your **webserver**, `sudo mysql -h  -u  -p`


```
sudo mysql -h 172.31.94.254 -u webaccess - tooling < tooling-db.sql

OR

sudo mysql -h 172.31.94.254 -u webaccess -p

```

24. Run this command on either your database server or webserver

```
sudo mysql
show databases;
use tooling;
show tables;
select * from users;
SELECT user , host , plugin from mysql.user;

```

25. You will find the login details to the website; the username: `admin` and password: `admin`

![4](https://user-images.githubusercontent.com/101978292/215033025-dcf1becb-b20c-448a-868a-943898877a96.jpg)

26. Finally, open the website in your browser with the **public IP/login.php** of the webserver and make sure you can login into the websute with myuser user.

*Remove the apache default page which resides on your public ip, so that the public ip redirects to your tooling website*

```
sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.backup

```

![Screenshot 2023-01-26 121613](https://user-images.githubusercontent.com/101978292/215042582-f370625a-fbd0-4b8b-bd30-193f05d67afa.jpg)

*Restart Apache `sudo systemctl restart httpd`*

![Screenshot 2023-01-26 121339](https://user-images.githubusercontent.com/101978292/215033197-afdb6b7a-5897-400a-aad2-f3ca2e6a8cb2.jpg)


![Screenshot 2023-01-26 132853](https://user-images.githubusercontent.com/101978292/215033286-2884c9d9-88e2-404e-a80a-13e8c3cdd450.jpg)
