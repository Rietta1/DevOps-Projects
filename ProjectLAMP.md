# [1) WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS]


***

#### 1. [A technology stack is a collection of frameworks and tools used in the creation of a software product. This collection of frameworks and technologies has been carefully selected to operate together to create a well-functioning program. They are abbreviations for individual techno]


#### 2. AWS account setup and provisioning of an Ubuntu Server



#### 3. Steps
1. Signed up for an AWS account.
2. Logged in as IAM user
3. In the VPC console, I create the Security Group
4. Launched an EC2 instance
5. I selected the Ubuntu free tier instance
6. I set the required configurations (Enabled public IP, security group, and key pair)
![Step 3 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/3cad2552-551d-4d12-82dd-ebf617f74154/1751db9e-1409-4109-acd8-9eaba5082a4e.png?crop=focalpoint&fit=crop&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1366%3A625)


#### 4.  INSTALLING APACHE AND UPDATING THE FIREWALL
 Steps


 1. Install Apache using Ubuntu’s package manager ‘apt', Run the following commands: To update a list of packages in package manager:
   ```
    sudo apt update
```
![Step 5 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/44817e91-237e-4feb-afac-4e4aa2313a88/f3c51316-e765-4f33-9e4c-d050c8ec9324.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 2. To run apache2 package installation:
```
    sudo apt install apache2
```
3. Next, verify that Apache2 is running as a service in the OS. run:
```
    sudo systemctl status apache2
```
4. The green light indicates Apache2 is running.
![Step 6 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/4852bfb3-3ba9-4a77-ad3f-be0cdc965337/61d45169-335e-4c82-a8a9-fe2a0e82bcc2.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 6. Open port 80 on the Ubuntu instance to allow access from the internet.
7. Access the Apache2 service locally in our Ubuntu shell by running: 
**curl http://localhost:80** or **curl http://127.0.0.1:80** This command would output the Apache2 payload indicating that it is access
![Step 7 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/7ddd0f31-4667-43aa-ba08-209c17bebd25/b7315c94-c8c4-4b4c-807b-5bca42f49b3d.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1138%3A546)


#### 5. INSTALLING MYSQL
Steps
In this step, I install a Database Management System (DBMS) to be able to store and manage data for the site in a relational database.


 1. Run ‘apt’ to acquire and install this software, run:
```
 sudo apt install mysql-server
```
2. Confirm installation by typing Y when prompted.

![Step 9 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/4a943496-17cf-490f-8a1e-31e5a2e104db/25493e85-f2ed-45fe-aa93-58e218680d4c.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 3. Once installation is complete, log in to the MySQL console by running: 
```
sudo mysql
```
![Step 10 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/16b64e8b-2dd1-4bde-9830-343a8d39b61b/77a53f39-670a-4c21-8e01-e44ec142975f.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 4. Next, run a security script that comes pre-installed with MySQL, to remove some insecure default settings and lock down access to your database system. run: 


 
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password1';
```
![Step 12 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/0f154c29-fbb1-4fc8-9b32-40de18c4fd3e/21e4c155-8766-453e-8073-aae30a34ef3c.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 5. Run the interactive script by typing:
```
 sudo mysql_secure_installation
```
and following the instructions.
6. Next, test that login to MySQL console works. Run: 
```
sudo mysql -p
```
![Step 13 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/a328d108-9cc2-4a92-8e00-a8cfa8ef55fd/87585a1f-c85a-479f-b9f5-257f57a93937.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 7. Type exit and enter to exit the console.


#### 6. INSTALLING PHP
Steps
1. To install these 3 packages at once, run:
```
sudo apt install php libapache2-mod-php php-mysql
```
![Step 15 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/4370d8a4-88ce-41f2-b0c9-c9cb2154f490/7ee68fa1-2500-4fbf-b7c4-1994dd6b893e.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 2. After installation is done, run the following command to confirm your PHP version: **php -v**
![Step 16 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/63498a43-a393-49bd-b57d-d9c4c074c8b2/3cb2079b-0474-4559-a03d-641262d9641d.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 3. At this point, your LAMP stack is completely installed and fully operational.



#### 7.   CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
 Steps
1. Setting up a domain called projectlamp. Create the directory for projectlamp using ‘mkdir’. Run:
```
 sudo mkdir /var/www/projectlamp
```

![Step 18 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/d1e5ea44-76bd-45ab-8e5a-460f0e9dbebf/c2675575-a887-44e9-a6fa-2b814ae5ccfc.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 2. assign ownership of the directory with your current system user, run:
``` sudo chown -R $USER:$USER /var/www/projectlamp
```



 3. Next, create and open a new configuration file in Apache’s sites-available directory. Type: sudo vi /etc/apache2/sites-available/projectlamp.conf

![Step 20 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/a781f32f-c356-4efe-8fd3-efd300647b03/89bc8689-8be5-4b1f-938c-a61664b87ca1.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 4. This will create a new blank file. Paste in the following bare-bones configuration by hitting on I on the keyboard to enter the insert mode, and paste the text:





 ```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
</VirtualHost >
```


![Step 22 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/5b7ff707-58c1-4d1b-a88a-0cd0e3500fbd/bfc95efb-1207-4574-b257-32339d43f9fd.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1354%3A556)


5. To save and close the file. Hit the esc button on the keyboard, Type Type:wq! w for write and q for quit and Hit ENTER to save the file.


 6. use the ls command to show the new file in the sites-available directory:
 ```
    sudo ls /etc/apache2/sites-available
```
![Step 24 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/0382c715-0efc-4fe7-859c-bc352314f58b/40cfc760-80f3-4cd3-add8-f93f6c41d40a.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


7. Next, use a2ensite command to enable the new virtual host:
 ```
    sudo a2ensite projectlamp
```
![Step 25 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/60d1e4af-c033-498a-93ef-05c948c73e0d/f06a592b-d3b4-4ddb-a6eb-1c2c6cae6e4c.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 8. Reload Apache so these changes take effect: 
```
   $ sudo systemctl reload apache2
```
![Step 26 screenshot](https://images.tango.us/workflows/ccf0ed53-ebe6-491b-a20c-5ffdeec18005/steps/f617069b-94a9-4612-9e57-f2273f458943/4392efc7-6d91-4abd-a6a3-063882d7120e.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)









 9. Finally, reload Apache so these changes take effect: 
```
 $ sudo systemctl reload apache2
```
![Step 27 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/0981bd85-3bda-475c-88e5-6b771e759b99/dfa9e971-22d4-4663-bf5d-5c3f956e3843.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 10. Next, use a2ensite command to enable the new virtual host:
 ```
  $ sudo a2ensite projectlamp
```
![Step 28 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/060de356-9144-4549-8b2e-637ab0c95ff8/c21d907d-bb7e-4183-8d28-2d1c2520c54a.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


 11. Disable the default website that comes installed with Apache. type:
```
   $ sudo a2dissite 000-default
```
![Step 29 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/de2abbd4-b4cf-420d-96cb-d6f9eedabb52/31592462-ea44-4ee7-a6a9-d11a3079ecd1.png?crop=focalpoint&fit=crop&fp-x=0.4916&fp-y=0.4317&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


12. Ensure your configuration file doesn’t contain syntax errors, run: 
```
    sudo apache2ctl configtest
```
![Step 30 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/1e435298-d8e5-4ab8-8cfa-396bcf461bb9/9a029303-0d59-46d4-802e-f29e357f75e7.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


13. Finally, reload Apache so these changes take effect: 
```
    sudo systemctl reload apache2
```
![Step 31 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/1b8c5182-8d12-4077-8cd1-b57df908a3ca/223e70c8-8b3f-4fe7-a4db-90c19c136b74.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=888%3A578)


  14. The website is active, but the webroot/var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

* Type and run :

```
 sudo vi /var/www/projectlamp/index.html
```

see my input :


![Step 32 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/875bb025-33e9-4d2a-bd2a-b8600d84a299/67ff46e2-9f8a-4153-a93d-b1741f047899.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1356%3A558)


 OR

 14. The website is active, but the webroot/var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

* Type and run :




```
    sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```

![Step 34 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/8cfd0449-be52-4b80-a683-3ddb9323905f/639f01f7-b4b8-41e9-abe0-24996637632e.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1356%3A558)


 15. Reload the public IP to see changes to the apache2 default page.


#### 8.   ENABLE PHP ON THE WEBSITE
 Steps
```
1. With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. To make index.php file tak precedence need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive.
```


 This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. 

```
Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
```


 2. In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

```
   sudo vim /etc/apache2/mods-enabled/dir.conf
```



<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

![Step 39 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/07b3454d-f5e7-4c62-80cd-8965af15e804/1bf3dacd-0f53-4449-ab7b-6193b9c105ae.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1363%3A549)


3. After saving and closing the file, you will need to reload Apache so the changes take effect:

```
 sudo systemctl reload apache2
```



4.  Finally, we will create a PHP Script to test if the PHP is correctly installed and configured on our Server.
 Now that we have a custom location to host our website's files and folders, 


5. we'll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
* Create a new file named index.php inside our custom web root folder:

```
 vim /var/www/projectlamp/index.php
```


6. This will open a blank file. Add the following text, which is valid PHP code, inside the file:

```
<?php
phpinfo();
```
![Step 43 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/9fb10c6e-b9c3-4dc4-a60b-c23d2b8c13c9/3d18db69-f46d-47f1-b386-f262898effcf.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1366%3A560)


7.  Save and close the file, then refresh the page to see changes.
![Step 44 screenshot](https://images.tango.us/workflows/f5bb192a-e935-4faa-bce3-6f7ee92f44ee/steps/c17cfd4f-2e40-4784-98b4-60ad29f416b9/117560c7-8089-4f5f-9883-89027590cd6f.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&fp-z=1.0000&w=1200&mark-w=0.2&mark-pad=0&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n&ar=1365%3A696)


8.  This page provides information about your server from the perspective of PHP. It is useful for debugging and ensuring that your settings are being applied correctly.


9.  If you can see this page in your browser, then your PHP installation is working as expected..


10.  After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:

\`\`\`

sudo rm /var/www/projectlamp/index

\`\`\`


