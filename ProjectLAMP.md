## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
### ASW account setup and provisioning an Ubuntu Server
#### Steps
1. Signed up for an AWS account.
2. Logged in as IAM user
3. In the VPC console, I create Security Group

4. Launched an EC2 instance
5. I selelected the Ubuntu free tier instance
6. I set the required configurations (Enabled public IP, security group, and key pair) and finally launched the instance.

7. Next I SSH into the instance using gitbash
8. In the Terminal, I typed cd Downloads to navigate to the locxcation of my key-pair.
9. Inside the Downloads directory, I connect to my instance using its Public DNS.

### INSTALLING APACHE AND UPDATING THE FIREWALL
#### Steps
1. Install Apache using Ubuntu’s package manager ‘apt', Run the following commands: To update a list of packages in package manager:
**sudo apt update**

2. To run apache2 package installation:
**sudo apt install apache2**
3. Next, verify that Apache2 is running as a service in the OS. run:
**sudo systemctl status apache2**
4. The green light indicates Apache2 is running.

6. Open port 80 on the Ubuntu instance to allow access from the internet.
7. Access the Apache2 service locally in our Ubuntu shell by running: 
**curl http://localhost:80** or **curl http://127.0.0.1:80** This command would output the Apache2 payload indicating that it is accessible locally in the Ubuntu shell.
8. Next, test that Apache HTTP server can respond to requests from the Internet. Open a browser and type the public IP of the Ubutun instance: **http://53.273.236.124/:80** This outputs the Apache2 default page.




### INSTALLING MYSQL
#### Steps
In this step, I install a Database Management System (DBMS) to be able to store and manage data for the site in a relational database.
1. Run ‘apt’ to acquire and install this software, run: **sudo apt install mysql-server**
2. Confirm intallation by typing Y when prompted.
3. Once installation is complete, log in to the MySQL console by running: **sudo mysql**

4. Next, run a security script that comes pre-installed with MySQL, to remove some insecure default settings and lock down access to your database system. run: 
**ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';** then exit MySQL shell by typing exit and enter.
5. Run interactive script by typing: **sudo mysql_secure_installation** and following the instrustions.
6. Next, test that login to MySQL console works. Run: **sudo mysql -p**

7. Type exit and enter to exit console.




### STEP 3 — INSTALLING PHP
#### Steps
1. To install these 3 packages at once, run:
**sudo apt install php libapache2-mod-php php-mysql**

2. After installation is done, run the following command to confirm your PHP version: **php -v**
4. At this point, your LAMP stack is completely installed and fully operational.




### STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
#### Steps
1. Setting up a domain called projectlamp. Create the directory for projectlamp using ‘mkdir’. Run: **sudo mkdir /var/www/projectlamp**
2. assign ownership of the directory with your current system user, run: **sudo chown -R $USER:$USER /var/www/projectlamp**
3. Next, create and open a new configuration file in Apache’s sites-available directory. Tpye: **sudo vi /etc/apache2/sites-available/projectlamp.conf**
4. This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
**<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>**

  5.To save and close the file. Hit the esc button on the keyboard, Type :, Type wq. w for write and q for quit and Hit ENTER to save the file.
  6. use the ls command to show the new file in the sites-available directory: **sudo ls /etc/apache2/sites-available**

   7. Next, use a2ensite command to enable the new virtual host: **sudo a2ensite projectlamp**
  8. Disable the default website that comes installed with Apache. type: **sudo a2dissite 000-default**
  9. Esure your configuration file doesn’t contain syntax errors, run: **sudo apache2ctl configtest**
  10. Finally, reload Apache so these changes take effect: **sudo systemctl reload apache2**

  12. The website is active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:
**sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html**
 13. Relaod the public IP to see changes to the apache2 default page.





### STEP 5 — ENABLE PHP ON THE WEBSITE
#### Steps
1. With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. To make index.php file tak precedence need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive.
2. Run: **sudo vim /etc/apache2/mods-enabled/dir.conf** then:
**<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>**
4. Save and close file.
5. Next, reload Apache so the changes take effect, type: **sudo systemctl reload apache2**
6. Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server. Create a new file named index.php inside the custom web root folder, run: **vim /var/www/projectlamp/index.php**
7. This will open a blank file. Add the PHP code: 
**<?php
phpinfo();**
8. Save and close the file, then refresh the page to see changes.
9. 