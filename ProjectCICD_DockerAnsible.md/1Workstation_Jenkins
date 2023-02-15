## WEBAPPS WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION and CONTINOUS DEPLOYMENT. 
### DevOps Automation

DevOps automation refers to the use of tools, processes, and systems to automate various aspects of software development and IT operations. The goal of DevOps automation is to improve efficiency, speed up the software delivery process, and ensure consistent and reliable operations. Some examples of DevOps automation include continuous integration and delivery (CI/CD), infrastructure as code (IaC), configuration management, and monitoring and logging.

According to Circle Continuous integration (CI) is a software development method that accelerates development while assuring the quality of the code that teams deliver. Developers continuously commit code in tiny increments (at least daily, if not multiple times per day), which is then automatically built and tested before being merged into the common repository.

### INTRODUCTION TO JENKINS

#### Task
**Build and Lunch the Webapps site through a CICI pipline. This pipeline consists of a Workstation Server, Application Server and a Production server, by which the task is configured automatically to publish source code updates from GIT to Application Server and then to the Production Server.**

SERVER REQUIRMENTS:
- JAVA
- GIT
- WGET
- MAVEN
- JENKINS

![Screenshot 2023-02-08 012722](https://user-images.githubusercontent.com/101978292/217397916-0c21b85c-73fa-449a-b23a-a810280d2221.jpg)


#### Step 1- Install Jenkins 
1. Create an AWS EC2 server based on Redhat9 Server and name it "Workstation Server"
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

#### Step 3- Install Maven
1. Install Wget, since redhat9 doesnt seem to have it installed `sudo yum install wget`

2.  Download Maven compressed file
Go to this website   ,copy the link to tar.gz and paste it on your server with sudo wget infront of it, 
```
 sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.0/binaries/apache-maven-3.9.0-bin.tar.gz

ls

```
- unzip the file just downloaded to your server using `tar -zxvf`
```
tar -zxvf  apache-maven-3.9.0-bin.tar.gz

```
- install maven
```
sudo yum install maven -y

```
-Copy the unzipped file to `/opt`

```
sudo cp -r apache-maven-3.9.0 /opt

```
3. Declare varables of java and Maven, for jenkins to easily access when we run the pipeline and build. Open `.bash_profile`

```
sudo vi .bash_profile

```
Paste this delarations underthe line 

```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
MAVEN_HOME=/opt/apache-maven-3.9.0/
M2=/opt/apache-maven-3.9.0/


PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME:$MAVEN_HOME:$M2


export PATH

```
#### Step 4- Install Git
1. install git
```
sudo yum install git -y
```

2. Add your git profile to your server
```
git config --global user.name "Rietta1"
git config --global user.email "yourgitemail@gmail.com"

```

3. Create a directory for your git and initalize it

```
git init Project1

cd Project1

```
4. Add the repository you are to pull code changes from and build

```
git remote add origin https://github.com/Rietta1/itern.git
git clone https://github.com/Rietta1/itern.git

```

5. Then list `ll` and go into the repo installed `cd itern`

#### Step 4- Install Jenkins 
1. The installations usually change, you can use this link to cross check: https://www.jenkins.io/doc/book/installing/linux/

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

sudo yum upgrade

sudo yum install jenkins
sudo systemctl daemon-reload


```


2. Make sure Jenkins is up and running: 

```

sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

```

![2](https://user-images.githubusercontent.com/101978292/217398295-1da2b8e5-ed3d-4548-9ed9-590fe0202cd5.jpg)


3. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group
4. Next, setup Jenkins. From your browser access `http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080` You will be prompted to provide a default admin password

![3](https://user-images.githubusercontent.com/101978292/217398321-9491c95d-ef13-4f82-b9b3-1418946836c1.jpg)


5. Retrieve the password from your Jenkins server: 
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

```

![4](https://user-images.githubusercontent.com/101978292/217398509-208ac4a1-3977-4092-bd1f-d24a2ee50ab5.jpg)

6. Copy the password from the server and paste on Jenkins setup to unlock Jenkins.
7. Next, you will be prompted to install plugins – **choose install suggested plugins**


![5](https://user-images.githubusercontent.com/101978292/217398696-e4f469a5-71a2-4296-bed5-bf0a8636e18f.jpg)
![6](https://user-images.githubusercontent.com/101978292/217398705-9584cdaa-baf5-4c98-b1bd-b61a322d3395.jpg)


8. Once plugins installation is done – create an admin user and you will get your Jenkins server address. **The installation is completed!**

![7](https://user-images.githubusercontent.com/101978292/217398739-039f1102-af55-48af-bb4f-a0568de3987d.jpg)

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

2. Go to Jenkins web console, click **New Item** and create a **Freestyle project** name it Project1 and click OK
3. Connect your GitHub repository, copy the repository URL from the repository
4. In configuration of your Jenkins freestyle project under Source Code Management select **Git repository**, provide there the link to your Itern GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![9](https://user-images.githubusercontent.com/101978292/217399139-b719a731-55f6-4340-8ec3-f4ba79957d2b.jpg)

![10](https://user-images.githubusercontent.com/101978292/217399209-47c65df8-2963-47a9-a0cc-89e54727ad36.jpg)

5. Save the configuration and let us try to run the build. For now we can only do it manually.
6. Click **Build Now** button, if you have configured everything correctly, the build will be successfull and you will see it under **#1**
7. Open the build and check in **Console Output** if it has run successfully.


![12](https://user-images.githubusercontent.com/101978292/217399432-7691320a-902d-4302-96a0-616596a9acd5.jpg)


8. Click **Configure** your job/project and add and save these two configurations:

``` 

Under **Post Build Actions** select Archieve the artifacts and enter `**` in the text box.
```

12. By default, the artifacts are stored on Jenkins server locally: `ls /var/lib/jenkins/jobs/webapps.war_github/builds/<build_number>/archive/`

13. 
go to the server and check for the  build 
 
`cd /var/lib/jenkins` and find the webapp.war file in the workspace directory then cd into target, 


