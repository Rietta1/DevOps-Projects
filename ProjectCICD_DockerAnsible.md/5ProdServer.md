## WEBAPPS WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION and CONTINOUS DEPLOYMENT. 
### DevOps Automation

DevOps automation refers to the use of tools, processes, and systems to automate various aspects of software development and IT operations. The goal of DevOps automation is to improve efficiency, speed up the software delivery process, and ensure consistent and reliable operations. Some examples of DevOps automation include continuous integration and delivery (CI/CD), infrastructure as code (IaC), configuration management, and monitoring and logging.


### INTRODUCTION TO PRODUCTION SERVER SERVER

#### Task
**Build,Conternarize and Lunch the Webapps site through a CICI pipline. This pipeline consists of a Workstation Server pulling the website build from git to the Application were it is containarized by docker to Production with Ansible** 

SERVER REQUIRMENTS:
- Java
- Tomcat
- Wget
- Docker

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
sudo vi .bash_profile

```
- Paste this delarations underthe line 

```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
CATALINA_HOME=/opt/apache-tomcat-10.0.27/
CATALINA_BASE=/opt/apache-tomcat-10.0.27/


PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME:$CATALINA_HOME:$CATALINA_BASE


export PATH


```
#### Step 4- Docker Installation

1. Install Docker
```
sudo dnf check-update

sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo dnf install docker-ce docker-ce-cli containerd.io

sudo systemctl start docker
sudo systemctl status docker

```

2. Docker is a group, so go to groups
```
cat /etc/group
```
3. Add the ec2-user to the group `docker`

```
sudo usermod -aG docker ec2-user

groups ec2-user

systemd-journal docker
sudo systemctl restart docker
sudo systemctl enable docker
sudo systemctl status docker

```
4. To use a docker, we need 3 things
- create a container[that is the docker that we installed]
- pull the latest tomcat image from the docker hub into docker
-  we move the image into that our container


5. Create Dockerfile 

```
touch Dockerfile

```
Add this to the Dockerfile `sudo vi Dockerfile`

```
FROM tomcat:latest 

#pull the latest tomcat image

COPY ./webapp.war /usr/local/tomcat/webapps

#copy webapp.war from homepage to the container
#Copy is a container command

RUN cp -r /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps

#copy all the files in webapps.dist file in the container to webapps in the container bcos the webapps of the docker container always comes empty

```
*right now you can run the commands for creating an image  `sudo docker build -t iternimage .`  and container: `sudo docker run -d --name IternCon -p 8080:8080 Internimage` manually on the server but we are going to put it in jenkins for it to be automated. Also this is the code to combine the command of pulling the latest image from docker hub into docker then create a container and move the image into the container `docker run - d --name IternCon -p 8080:8080 tomcat:latest`*

7. Go to the folder `sudo cd /opt/apache-tomcat-10.0.27/bin`
were tomcat is installed and start it with: `./startup.sh`

8. Install  netstat to enable you check what process is using your server

```
sudo yum install net-tools 
```

9. check if port 8080 is in use, 

```
 sudo netstat -tnlp | grep :8080
```

10. If yes kill it `sudo kill -9 process no`

```
sudo kill -9 6424
```