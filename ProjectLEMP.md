# [ WEB STACK IMPLEMENTATION (LEMP STACK)](https://app.tango.us/app/workflow/03f2d5c5-04ac-4c75-85a6-79fde1e89758?utm_source=markdown&utm_medium=markdown&utm_campaign=workflow%20export%20links)



### 1.  Step 1 – Installing the Nginx Web Server

* To display web pages to our site visitors we will employ Nginx, a high-performance Web Server. We'll use the apt Package Manager to install this package.



### 2. Next, install Nginx by running:

```
 sudo apt install nginx -y
```

* To verify that nginx was successfully installed and running as a service in Ubuntu, run:

```
$ sudo systemctl status nginx
```

![Step 2 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/80e2134e-aa95-4643-b7d6-f9fd854fc39d/cd4aaee7-2997-4376-b3f2-c094b57e7c00.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1363%3A480)


### 3. [ if it is green and running, then you did everything correctly - you have just launched your first web Server in the Cloud! ]




### 4.  The server is running and we can access it locally and from the Internet but to access it from the internet port 80 must be open to allow traffic from the internet in.


### 5.  To test that the server can be accessed from the internet, open a browser and type the following url with the public IP of your Ubuntu instance; 
**http://Public-IP-Address:80**
![Step 5 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/e29db57d-5a21-440c-bcd5-39934d4c0cd9/5155f41b-958e-4a0e-bf71-193f447bb1cd.png?crop=focalpoint&fit=crop&fp-x=0.5006&fp-y=0.0703&fp-z=1.2182&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


## 6.  STEP 2 — INSTALLING MYSQL
Steps
 To acquire and install SQL run:

```
  sudo apt install mysql-server
```
 in the terminal.
![Step 6 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/3e1a5102-d056-4fe7-8598-9e57f87c301f/e73a5b70-2330-48fe-892b-84509f33885a.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 7. Restart the service "sudo service mysql start" 
![Step 7 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/089ed1e2-c7fa-41fd-917d-0963ddcca544/498ead83-6fe8-4434-be7b-0bec543418e8.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 8.  At the prompt, confirm installation by typing Y and enter to proceed.
 Next, log into MySQL: 
```
sudo mysql
```
![Step 8 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/a7432458-cf73-4509-9ed4-3af765ffac4c/471540d1-5c8a-4b93-b506-1f70cf8f56b8.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 9.  Run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. run the following command: 
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
![Step 9 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/7fa6ae80-a079-478b-b172-d4f88c7716d0/5bbf7726-1743-4634-a842-cce878679bd2.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 10.  Exit SQL shell by typing **exit** 


### 11. When the installation is finished, it's recommended that you run a security script that comes pre-installed with MySQL. This Script will remove some insecure default settings and lock down access to your database system. Start the interactive script by running:



### 12. 

```
  sudo mysql_secure_installation
```


### 13. This will ask if you want to configure the VALIDATE PASSWORD PLUGIN. Answer Y for "yes" or anything else to continue without enabling. If you answer "yes", you'll be asked to select a level of password validation. 
```
Keep in mind that if you enter 2 for the strongest level you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special charachers, or which is based on common dictionary words.

Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN, your Server will next ask you to select and confirm a password for MySQL root user. This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure.

If you enabled password validation, you'll be shown the password strength for the root password you just entered and your Server will ask if you want to continue with that password. If you are happy with your current password, enter Y for "yes" at the prompt.

For the rest of the questions, press Y and hit ENTER key at the prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respect the changes you have made.
```


### 14. As seen below:

![Step 14 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/46424eed-5ab1-453a-b47f-bb9aed046e11/1d915433-47b3-4a03-9952-2cfb7090d382.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 15.  When done, test to know if login to the console is possible. type: 
``` 
   sudo mysql -p
```
* This would prompt you to enter the root user password. Enter the chosen password and enter.
* type exit to exit MySQL console.


## 16. STEP 3 – INSTALLING PHP
 Steps
You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the Web Server.
```
While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the Web Server. This allows for a better performance in most PHP-based websites, but it requires additional configuration. You'll need to install php-fpm, which stands for "PHP fastCGI Process Manager", and tell Nginx to pass PHP requests to this software for processing. Additionally you'll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependenes.
```


### 17. To install these 2 packages at once, run :

```
 sudo apt install php-fpm php-mysql
```

When prompted, type Y and press ENTER to confirm the installation. You now have your PHP components installed. Next, you will configure Nginx to use them
![Step 17 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/2fe20b13-b352-4f1d-b96d-66997b7988fd/e35190a1-1134-4982-8b97-465ba43cc2c1.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


## 18. STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR
Steps
When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. 

```
We will use projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.
```


### 19. Now that we have PHP components installed. Next, I will configure Nginx to use them.
 Create the root web directory for your_domain as follows:
```
  sudo mkdir /var/www/projectLEMP
```
![Step 19 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/c990f6f4-97a7-4698-a2cb-a40a45baa8c4/9d7f711d-09db-4542-8280-4d3fb0356fef.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 20. Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

```
 sudo chown -R $USER:$USER /var/www/projectLEMP
```

![Step 20 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/a331e0f4-6f5d-40d1-bc60-20fb69a63329/e2e45828-b613-4ca6-a522-52baa2feaa1f.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 21. Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:

```
 sudo vi /etc/nginx/sites-available/projectLEMP
```
This will create a new blank file. Paste in the following bare-bones configuration:
````
```
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```
````


### 22. As seen :

![Step 22 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/f2738ca2-41ed-4b46-9e63-02cf58e4a8a6/e4067f09-c130-4c5b-a37c-ae7155d6646d.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1362%3A450)


### 23. Here’s what each of these directives and location blocks do:
```
listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.

root — Defines the document root where the files served by this website are stored.

index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.

server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.

location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

location ~ .php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

location ~ /.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.

When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.
```


### 24. Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

```
 sudo ln -s /etc//nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
![Step 24 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/903a5808-808e-4ceb-8b6d-957cffddeae0/1b99611b-7114-48a0-8442-20f483bc8a9b.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 25. This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:

```
 sudo nginx -t
```


### 26. You shall see the following message:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
![Step 26 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/fa4ca562-7cdb-474e-bf45-4d69db71d381/f82fcf51-9f08-4085-a67f-dcf2833345af.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 27. We also need to disable the default Nginx host that is currently configured to listen on port 80, for this run:

```
 sudo unlink /etc/nginx/sites-enabled/default
```

When you are ready, reload Nginx to apply the changes:

```
 sudo systemctl reload nginx
```
![Step 27 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/7b49e26f-ecf4-4de3-9d85-c14892b63fd1/e9028ee4-9d87-4f3f-a035-7ef57201acbb.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 28. [Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:]

### 29.  

```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

```
![Step 29 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/b1eb7f3b-b8ab-490d-863e-e9f714d209a5/1167ce7f-7367-413d-9539-60291ace141b.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 30.  Open a browser and try to open the website URL using IP address: **http://Public-IP-Address:80**
![Step 30 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/7c45d053-c0f4-479f-8065-91b33f6d52db/fb0b51d0-4a08-41b0-a851-255475646af2.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1352%3A695)


### 31. Leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default


### 32. Your LEMP stack is now fully configured. In the next step, we’ll create a PHP script to test that Nginx is in fact able to handle .php files within your newly configured website.



## 33.  STEP 5 – TESTING PHP WITH NGINX
 ### Steps
Your LEMP stack should now be completely set up.

At this point, your LAMP stack is completely installed and fully operational.

You can test it to validate that Nginx can correctly hand .php files off to your PHP processor.



### 34. You can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:

```
   vi /var/www/projectLEMP/info.php
```


### 35. Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

```
<?php
phpinfo();
```
![Step 35 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/08f997f7-b355-4c85-b7c7-8e907fde85c7/6534e540-05c0-4d3d-ba49-44849998a1c0.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1365%3A454)


### 36. You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:

```
http://`server_domain_or_IP`/info.php
```

You will see a web page containing like this:
![Step 36 screenshot](https://images.tango.us/workflows/03f2d5c5-04ac-4c75-85a6-79fde1e89758/steps/0a646ba7-2c6a-466b-b135-a3b4faecc51e/aa464f6c-af0a-4d53-a184-128ba8ac427c.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1365%3A625)



***

### 37. After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:


### 38. 

```
 sudo rm /var/www/your_domain/info.php
```

You can always regenerate this file if you need it later.

![Step 38 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/c1880682-a76b-4d9c-9439-f80d3389e6ed/bbdfe5ca-b713-445a-9490-440b9eb93d62.png?crop=focalpoint&fit=crop&fp-x=0.4908&fp-y=0.4184&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 39.
### STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
#### Steps


### 40. The goal in this step is to create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.


### 41. Because the native MySQL PHP library mysqlnd currently doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. 


### 42.  We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.


### 43. I will be creating a database named example_database and a user named example_user.
Connect to the MySQL console using the root account: 
```
  sudo mysql -p
```

![Step 43 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/bc3b68cc-0c21-4ec5-a017-cafb1bc664b4/e572453b-4dce-4032-b14f-85ead5aa12ac.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 44. To create a new database, run the following command from your MySQL console:

```
mysql> CREATE DATABASE `example_database`;
```
![Step 44 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/b252fd32-131a-43bc-9c2d-db207c902f08/6e2d9fcb-376a-4674-952b-1abcfb4df16f.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 45.  Next, create a new user and grant him full privileges on the database. The following command creates a new user named example_user, using:


### 46. 

```
mysql_native_password as default authentication method. Run: **CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password@';
```
![Step 46 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/66dbd480-f7ae-4f99-aa83-0be2c27f2efb/66bb1df6-6afb-48f9-94e5-5167a14d7243.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 47.  Next, create a new user and grant him full privileges on the database. The following command creates a new user named example_user, using mysql_native_password as the default


### 48. 

```
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%'WITH GRANT OPTION;
```
![Step 48 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/25524a91-e111-47e9-9228-fa6218c57dc8/2726e34f-d7e0-49f6-bcce-6bb1398b38c6.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 49. This will give the example_user user full privileges over the example_database database while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with:

```
mysql> exit
```


### 50. You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

```
   sudo mysql -p
```

![Step 50 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/cc5cc273-edab-4a57-a3f7-5a97cc689590/eaf9f550-5268-4b9d-b45d-c913bbd965a1.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 51. Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. 

After logging in to the MySQL console, confirm that you have access to the example_database database:

```
mysql> SHOW DATABASES;
```
![Step 51 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/3457f33d-4bc3-4ece-a863-fcaa102043a7/982d5ed1-c9d3-44f8-a822-72ac6b738d0b.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 52. Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:
``` 
CREATE TABLE example_database.todo_list (
item_id INT AUTO_INCREMENT,
content VARCHAR(255),
PRIMARY KEY(item_id)
);
```
![Step 52 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/1b3ac59c-aab3-40c9-8eab-24196cddf933/74e37172-7a75-4bb4-93f0-796b258733ec.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 53. Insert a few rows of content in the test table. Repeat the next command a few times, using different VALUES:

```
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
```
![Step 53 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/deaec39a-154f-4972-8571-894cf520bb9f/fed59809-c315-42c0-9625-e67632958106.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 54. To confirm that the data was successfully saved to your table, run:

```
mysql>  SELECT * FROM example_database.todo_list;
```

You’ll see the following output:
![Step 54 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/6fbae9ce-e40c-4a1f-bf52-11822724fdbe/a9d65b4a-61c3-474b-a80f-38dc820b310d.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=871%3A576)


### 55.  After confirming that you have valid data in your test table, you can exit the MySQL console:

```
mysql> exit
```


### 56. Next, create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. Use:


```
  vi /var/www/projectLEMP/todo_list.php
```
Copy this content into your todo_list.php script:
````
```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

```
````
![Step 56 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/0462827a-15e0-4122-b5d9-d18f4a5ba120/ac2902b2-5a98-4106-8cd2-d6a864801de1.png?crop=focalpoint&fit=crop&fp-x=0.4941&fp-y=0.4264&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1366%3A625)


### 57.  Save and close the file when you are done editing.
Access this page in the web browser by visiting the domain name or public IP address configured for the website, followed by /todo_list.php: **http://<Public_domain_or_IP>/todo_list.php**
![Step 57 screenshot](https://images.tango.us/workflows/0f70d4e7-596a-44a1-9fa9-151759f86e32/steps/2e1a6eb0-ef9f-444c-8021-249648111c22/7f324d43-8afa-4fc6-9987-e679d4c307a7.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1334%3A491)

.

