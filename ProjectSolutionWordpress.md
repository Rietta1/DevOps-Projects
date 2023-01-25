## Web-Solution-With-Wordpress

 WordPress (WP, WordPress.org) is a free and open-source content management system (CMS) written in PHP[4] and paired with a MySQL or MariaDB database. Features include a plugin architecture and a template system, referred to within WordPress as Themes. WordPress was originally created as a blog-publishing system but has evolved to support other web content types including more traditional mailing lists and forums, media galleries, membership sites, learning management systems (LMS) and online stores. 


To function, WordPress must be installed on a web server for it to work, either as a component of an Internet hosting service like WordPress.com or on a computer running the WordPress.org software package to act as a network host on its own.


**Three-tier Architecture**

 Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.




![1_lSIpD4-3C6F47yFBXjVvSQ](https://user-images.githubusercontent.com/101978292/214528322-b6d03c2e-789d-4d25-bd48-d4279913a47c.png)



![image](https://user-images.githubusercontent.com/85270361/134702570-9fe3ec31-4ba7-4545-8f47-1104eda0a93d.png)


#### 3-Tier Setup

* A Laptop or PC to serve as a client
* An EC2 Linux Server as a web server (This is where you will install WordPress)
* An EC2 Linux server as a database (DB) server

**A Three-tier Architecture** enables the independent upgrade or replacement of any one of the three layers.

The user interface is implemented on any platform, such as a desktop PC, smartphone, or tablet, as a native application, web app, mobile app, voice interface, and so on. It uses a standard graphical user interface with different modules running on the application server.

The relational database management system on the database server contains the computer data storage logic.

The middle tiers are usually multitiered.

Because the three are not physical but logical in nature, they may operate on various servers in both on-premises and software-as-a-service systems (SaaS).


The Three Tiers in a Three-Tier Architecture

**Presentation Tier**

* Occupies the top level and displays information related to services commonly available on a web browser or web-based application in the form of a graphical user interface (GUI).

* It constitutes the front-end layer of the application and the interface with which end-users will interact directly.

* This tier is often based on web development frameworks such as CSS or JavaScript and connects with other tiers by passing results to the browser and other tiers in the network via API calls.

**Application Tier**

* This tier, which is also known as the middle tier, business logic, or logic tier, is taken from the presentation tier.

* It controls the application’s core functionality by performing detailed processing and is usually coded in programming languages, such as Python, Java, C++, .NET, etc.

**Data Tier**

* Houses database servers where information is stored and retrieved.

* Data in this tier is kept independent of application servers or business logic, and is managed and accessed with programs, such as MongoDB, Oracle, MySQL, and Microsoft SQL Server.


 In this project, We will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively. The following steps will be followed to achieve our aim and objectives of this project.


### Part 1: Configure storage subsystem for Web and Database servers based on Red hat Linux OS.
#### Steps
1. Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.
2. Attach all three volumes one by one to your Web Server EC2 instance
3. Open up the Linux terminal to begin configuration of the instance. Use
```
 lsblk
 ```
  command to inspect what block devices are attached to the server. The 3 newly
created block devices are names **xvdf, xvdh, xvdg**
4. Use 
```
df -h
``` 

command to see all mounts and free space on your server
![pix 2](https://user-images.githubusercontent.com/74002629/182373755-c02f2da2-046b-40d0-b95e-c389fa3ce9e4.PNG)


5. Use gdisk utility to create a single partition on each of the 3 disks 
```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```

6. A prompt pops up, type **n** to create new partition, enter no of partition(1), hex code is **8e00**, **p** to view partition and **w** to save newly created partition.
7. Repeat this process for the other remaining block devices.
8. Type **lsblk** to view newly created partition.
![pix4](https://user-images.githubusercontent.com/74002629/182373794-69594381-2aeb-44f6-8b82-ac8565a82952.PNG)

9. Install **lvm2** package by typing: 
```
sudo yum install lvm2
``` 
Run 
```
sudo lvmdiskscan
```
 command to check for available partitions.

10. Create physical volume to be used by lvm by using the pvcreate command: 
```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```
11. To check if the PV have been created type: 
```
sudo pvs
```
![pix7](https://user-images.githubusercontent.com/74002629/182373892-afab86a7-1020-4c34-8be9-52c691330d68.PNG)

12. Next, Create the volume group and name it **webdata-vg**:
 ```
sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```
13. View newly created volume group type: 

```
sudo vgs
```
![pix8](https://user-images.githubusercontent.com/74002629/182373911-ac764044-c860-4b5e-9957-f1135dfe570f.PNG)

14. Create 2 logical volumes using lvcreate utility. Name them: **apps-lv** for storing data for the Website and **logs-lv** for storing data for logs.

```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

![pix9](https://user-images.githubusercontent.com/74002629/182373931-d9d3c292-f5c8-4147-950d-3aea3d77bc47.PNG)

Verify the entire setup
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk
``` 

15. Verify Logical Volume has been created successfully by running: 
```
sudo lvs
```
16. Next, format the logical volumes with ext4 filesystem: 

```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![pix11](https://user-images.githubusercontent.com/74002629/182375321-78581a9b-8389-403a-91ff-653f04164f0b.PNG)

17. Next, create mount points for logical volumes. Create **/var/www/html** directory to store website files: 
```
sudo mkdir -p /var/www/html
```
 and mount **/var/www/html** on apps-lv logical volume : 
 ```
 sudo mount /dev/webdata-vg/apps-lv /var/www/html/
 ```
![pix12](https://user-images.githubusercontent.com/74002629/182375326-619af95d-796d-4c85-8063-9588ff143aba.PNG)

18. Then create **/home/recovery/logs** to store backup of log data: 
```
sudo mkdir -p /home/recovery/logs
``` 
19. Use **rsync** utility to backup all the files in the log directory **/var/log** into **/home/recovery/logs** (It is important to backup all data on the /var/log directory because all the data will be deleted during the mount process) Type the following command: 
```
sudo rsync -av /var/log/. /home/recovery/logs/
```
20. Mount /var/log on logs-lv logical volume: 
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
21. Finally, restore deleted log files back into /var/log directory: 
```
sudo rsync -av /home/recovery/logs/. /var/log
```
22. Next, update **/etc/fstab** file so that the mount configuration will persist after restart of the server.
23. The UUID of the device will be used to update the /etc/fstab file to get the UUID type: 
```
sudo blkid
``` 
and copy the both the apps-vg and logs-vg UUID (Excluding the double quotes)
24. Type sudo 
```
vi /etc/fstab
```
 to open editor and update using the UUID you copied.

![pix13](https://user-images.githubusercontent.com/74002629/182375342-2c0713a4-946d-4e2c-a756-84472eb1ec34.PNG)

25. Test the configuration and reload the daemon: 
```
sudo mount -a`
sudo systemctl daemon-reload
```
26. Verify your setup by running `df -h`
![pix15](https://user-images.githubusercontent.com/74002629/182375405-7cf58fec-605c-41b9-b48e-bea89656a452.PNG)


### Part 2 -Install WordPress for connecting to a remote MySQL database server.

29. Update the repository: 

```
sudo yum -y update
```
30. Install wget, Apache and it’s dependencies: 

```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
31. Start Apache

```
sudo systemctl enable httpd
sudo systemctl start httpd
```

![pix17](https://user-images.githubusercontent.com/74002629/182375448-cdc35ab4-7f85-43f9-be40-b8e3419513c9.PNG)

32. install PHP and it’s depemdencies:

Follow this link to get the latest installation process as it changes from time to time :

https://www.tecmint.com/install-php-8-on-centos/

```
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module list php

sudo dnf module enable php:remi-8.0 -y

 php -v

```

33. Restart Apache: 

```
sudo systemctl restart httpd
```

34. Download wordpress and copy wordpress to **var/www/html**

```
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress /var/www/html/
```

![pix18](https://user-images.githubusercontent.com/74002629/182390571-8c367a9a-531b-44b2-b499-2ca2850286b5.PNG)

35. Configure SELinux Policies:

```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```

![pix19](https://user-images.githubusercontent.com/74002629/182390591-c618394d-4064-47e1-bc80-971665d5fcf8.PNG)

### Step 3 — Install MySQL on your DB Server EC2
36. Run the following:

```
sudo yum update
sudo yum install mysql-server
```

37. Verify that the service is up and running by using `sudo systemctl status mysqld`. If the service is not running, restart the service and enable it so it will be running even after reboot:

```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

![pix20](https://user-images.githubusercontent.com/74002629/182390616-7a7f9464-5df3-4997-8a1b-a3ce3ae712d3.PNG)


### Part 4 - Prepare the Database Server

27. Launch a second RedHat EC2 instance and name it **DB Server**

28. Repeat the same steps as for the Web Server, but instead of **apps-lv** create **db-lv** and mount it to **/db** directory instead of /var/www/html/.

* Attach all three volumes one by one to your Web Server EC2 instance
* Open up the Linux terminal to begin configuration of the instance. Use
```
 lsblk
 ```
  command to inspect what block devices are attached to the server. The 3 newly
created block devices are names **xvdf, xvdh, xvdg**
* Use

```
df -h
``` 

command to see all mounts and free space on your server


* Use gdisk utility to create a single partition on each of the 3 disks 

```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```

* A prompt pops up, type **n** to create new partition, enter no of partition(1), hex code is **8e00**, **p** to view partition and **w** to save newly created partition.
* Repeat this process for the other remaining block devices.
* Type **lsblk** to view newly created partition.


* Install **lvm2** package by typing: 

```
sudo yum install lvm2
``` 

Run 

```
sudo lvmdiskscan
```

 command to check for available partitions.

* Create physical volume to be used by lvm by using the pvcreate command: 

```
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
```

* To check if the PV have been created type:

```
sudo pvs
```


* Next, Create the volume group and name it **vg-database**:

 ```
sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```
* View newly created volume group type: 

```
sudo vgs
```


* Create 1 logical volumes using lvcreate utility. Name it: **db-lv** 

```
sudo lvcreate -n db-lv -L 20G vg-database
```

* Verify the entire setup

```

sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```

* Verify Logical Volume has been created successfully by running: 

```
sudo lvs
```

* Next, format the logical volumes with ext4 filesystem: 

```
sudo mkfs -t ext4 /dev/vg-databases/db-lv
```

* We need to create /db directory to store our database files**

```
$ sudo mkdir /db
```


* We need to create /home/recovery/logs to store backup of log data


```
$ sudo mkdir -p /home/recovery/logs
```


* We need to **Mount /db on db-lv logical volume**


```
sudo mount /dev/vg-database/db-lv /db
df -h
```

<img width="466" alt="db vl" src="https://user-images.githubusercontent.com/85270361/135603121-44ac41ac-b9e0-4f04-ada2-e8b3a262f134.PNG">


* We need to backup all the files in the log directory **/var/log into /home/recovery/logs** (This is required before mounting the file system)**

```
$ sudo rsync -r /var/log/ /home/recovery/logs/
```

* We need to **Mount /var/log on logs-lv logical volume**

```
$ sudo mount /dev/vg-database/logs-lv /var/log
```

* We need to restore log files back into /**var/log directory.**


```
$ sudo cp -r /home/recovery/logs/.* /var/log
```


* We need to update the `/etc/fstab` file so that the mount configuration will persist upon restart of the server.

* The UUID of the device will be used to update the `/etc/fstab` file to get the UUID type:

```
sudo blkid
```

 and copy the both the apps-vg and logs-vg UUID (Excluding the double quotes)

* Type 

```
sudo vi /etc/fstab
```

 to open editor and update using the UUID you copied.

Test the configuration and reload the daemon:

```
sudo mount -a`
sudo systemctl daemon-reload
```

Verify your setup by running 

```
df -h
```



### Step 6 — Configure DB to work with WordPress
38. Configure DB to work with Wordpress with the code below.

Log into mysql

```
sudo mysql
```
Change plugin to enable you create a secure user

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

```
Then **exit**

RUN Secure installation and follow the prompt

```
sudo mysql_secure_installation
```

Create database and user

```
mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>`
 OR
CREATE USER `myuser`@`%`
IDENTIFIED BY 'password';
GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
OR
'myuser'@'%';
FLUSH PRIVILEGES;
SHOW DATABASES;
SELECT user , host from mysql.user;
exit
```
![pix21](https://user-images.githubusercontent.com/74002629/182390638-84cd0f8d-66aa-4c9a-a3d9-17f9dad7f00a.PNG)

Set the binding address

```
sudo vi /etc/my.cnf
```

Add this script at the end of the exiting script

```
[mysqld]
bind.address=0.0.0.0
```

Restart service

```
sudo systemctl restart mysqld
```


### Step 6 — Configure WordPress to connect to remote database.
39. Make sure to open MySQL port 3306 on DB Server EC2. 
For extra security, you shall allow access to the DB server ONLY from your *Web Server’s IP address*, so in the Inbound Rule configuration specify source as /32

40. Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client

```
sudo yum install mysql
```

41. Open config.php and edit the db_name, user, password and host to your database details 

*note: for the host add your webserver private ip address*

```
sudo vi wp-config.php
```

Edit **DB_NAME** to `wordpress`, **DB_USER** to `myuser` and **DB_PASSWORD** to `password`
**DB_HOST** to `172.31.83.36`

42. Now disable apache by using this code, so that we can see the wordpress site

```
 sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup
 ```

 43. Verify that your webserver can communicate with your database **sudo mysql -u admin -p -h <DB-Server-Private-IP-address>**


```
sudo mysql -h 172.31.83.36 -u myuser -p
```

44. Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.

![pix26](https://user-images.githubusercontent.com/74002629/182393684-bb4357e0-14c2-44ba-80d2-eca86b5d7148.PNG)


45. Change permissions and configuration so Apache could use WordPress:
Run the following commands as it is needed for Red Hat  



```
sudo chown -R apache:apache /var/www/html/wordpress

sudo chcon -t httpd_sys_rw_content_t /var/www/html/ -R

sudo setsebool -P httpd_can_network_connect=1

sudo setsebool -P httpd_can_network_connect_db 1

```


 46. Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)
47. Try to access from your browser the link to your WordPress http:// < Web-Server-Public-IP-Address >/wordpress/

![pix28](https://user-images.githubusercontent.com/74002629/182393673-8c9cc21a-fee6-4c40-9ab5-034f968dafc5.PNG)
![pix29](https://user-images.githubusercontent.com/74002629/182393677-7204a1e6-3c1f-4b04-969c-5439762a4029.PNG)
  
