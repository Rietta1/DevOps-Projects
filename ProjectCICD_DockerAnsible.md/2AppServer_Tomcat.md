## WEBAPPS WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION and CONTINOUS DEPLOYMENT. 
### DevOps Automation

DevOps automation refers to the use of tools, processes, and systems to automate various aspects of software development and IT operations. The goal of DevOps automation is to improve efficiency, speed up the software delivery process, and ensure consistent and reliable operations. Some examples of DevOps automation include continuous integration and delivery (CI/CD), infrastructure as code (IaC), configuration management, and monitoring and logging.


### INTRODUCTION TO APPLICATION SERVER(TOMCAT)

#### Task
**Build and Lunch the Webapps site through a CICI pipline. This pipeline consists of a Workstation Server pulling the website from git to the Application Tomcat Server which has tomcat installed on it** 


SERVER REQUIRMENTS:
- JAVA
- TOMCAT

![Screenshot 2023-02-08 012722](https://user-images.githubusercontent.com/101978292/217397916-0c21b85c-73fa-449a-b23a-a810280d2221.jpg)


#### Step 1- Setup Server and create a user password
1. Create an AWS EC2 server based on Redhat9 Server and name it "App Server"
2. Update your server

```
sudo yum update -y
```
3. Create a password for the user on the server
```
sudo su
passwd ec2-user 
(book1)
```
- Authenticate the password by editting the configuration file `sshd_config` and change the `PasswordAthentication` to yes 

```
 vi /etc/ssh/sshd_config

```
- Restart the sshd file
```
systemctl restart sshd
systemctl enable sshd
systemctl status sshd

```
- switch back to ec2-user
```
su ec2-user
```
#### Step 2- Install JDK (Java development kit) since 

1. Jenkins is a Java-based application:

```
sudo yum install java-1.8.0-openjdk-devel -y
```
#### Step 3- Install Tomcat
1. Install Wget, since redhat9 doesnt seem to have it installed `sudo yum install wget`

2.  Download Tomcat compressed file
Go to this website   ,copy the link to tar.gz and paste it on your server with sudo wget infront of it, 
```
 sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.27/bin/apache-tomcat-10.0.27.tar.gz

ls

```
- unzip the file just downloaded to your server using `tar -zxvf`
```
 tar -zxvf apache-tomcat-10.0.27.tar.gz
 

```

- Copy the unzipped file to `/opt`

```
sudo cp -r apache-tomcat-10.0.27 /opt

```
3. Go to the folder `cd /opt/cd apache-tomcat-10.0.27/bin`
were tomcat is installed and start it with: `./startup.sh`

4. Declare varables of java and Tomcat, for jenkins to easily access when we run the pipeline and build. Open `.bash_profile`

```
sudo vi.bash_profile

```
- Paste this delarations underthe line 

```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
CATALINA_HOME=/opt/apache-tomcat-10.0.27/
CATALINA_BASE=/opt/apache-tomcat-10.0.27/


PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME:$CATALINA_HOME:$CATALINA_BASE


export PATH


```
#### Step 4- pubish over ssh to tomcat 


1. Go to global configuration and add the server information for publish over ssh

```
 SSH Servers name: app server
Hostname: 172.31.48.168
Username: ec2-user
remote directory: .
Passphrase / Password: book1
```
then go to Project1b Configuration *POST BUILD ACTION*

```
pubish over ssh to app server to Tomcat
project1b

[source files:] 
webapp/target/webapp.war

[Remove prefix:] 
webapp/target

[Remote directory]
.

[Exec command:]
 sudo cp ./webapp.war /opt/apache-tomcat-10.0.27/webapps
```

#### Step 5-  Configure Jenkins with the declared variables details. 

1. Go to Setup Global Tool Configuration and on the installations part, add

```
add java  
name :JAVA_HOME

JAVA_HOME: /usr/lib/jvm/java-1.8.0-openjdk

add git
name: git
Path to Git executable: /usr/bin/git

add maven
name:MAVEN_HOME
MAVEN_HOME:/opt/apache-maven-3.9.0/

```

2. Go to Jenkins web console, click **New Item** and clone Project1 or create a **Freestyle project** name it Project1b and click OK

3. Connect your GitHub repository, copy the repository URL from the repository

4. In configuration of your Jenkins freestyle project under Source Code Management select **Git repository**, provide there the link to your Itern GitHub repository and credentials (user/password) so Jenkins could access files in the repository.


![9](https://user-images.githubusercontent.com/101978292/217399139-b719a731-55f6-4340-8ec3-f4ba79957d2b.jpg)

![10](https://user-images.githubusercontent.com/101978292/217399209-47c65df8-2963-47a9-a0cc-89e54727ad36.jpg)

5. Click **Configure** your job/project and add and save these two configurations:

``` 

Under **Post Build Actions** select Archieve the artifacts and enter `**` in the text box.
```

6. Save the configuration and let us try to run the build. For now we can only do it manually.
7. Click **Build Now** button, if you have configured everything correctly, the build will be successfull and you will see it under **#2**
8. Open the build and check in **Console Output** if it has run successfully.


![12](https://user-images.githubusercontent.com/101978292/217399432-7691320a-902d-4302-96a0-616596a9acd5.jpg)




9. By default, the artifacts are stored on Jenkins server locally: `ls /var/lib/jenkins/jobs/webapps.war_github/builds/<build_number>/archive/`

10. 
go to the server and check for the  build 
 
`cd /opt/apache-tomcat-10.0.27/webapps` and find the webapp.war file in the workspace directory then cd into target, 

