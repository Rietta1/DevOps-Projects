## TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. 
### DevOps Automation

DevOps automation refers to the use of tools, processes, and systems to automate various aspects of software development and IT operations. The goal of DevOps automation is to improve efficiency, speed up the software delivery process, and ensure consistent and reliable operations. Some examples of DevOps automation include continuous integration and delivery (CI/CD), infrastructure as code (IaC), configuration management, and monitoring and logging.

According to Circle Continuous integration (CI) is a software development method that accelerates development while assuring the quality of the code that teams deliver. Developers continuously commit code in tiny increments (at least daily, if not multiple times per day), which is then automatically built and tested before being merged into the common repository.

### INTRODUCTION TO JENKINS

#### Task
**Modify the Project 8 architecture by adding a Jenkins server and configuring a task to automatically publish source code updates from Git to an NFS server.**

![Screenshot 2023-02-08 012722](https://user-images.githubusercontent.com/101978292/217397916-0c21b85c-73fa-449a-b23a-a810280d2221.jpg)


#### Step 1- Install Jenkins server
1. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
2. Install JDK (Java development kit) since Jenkins is a Java-based application:

```
sudo apt update
sudo apt install default-jdk-headless
```
3. Install Jenkins:
The installations usually change, you can use this link to cross check: https://www.jenkins.io/doc/book/installing/linux/

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

```

![1](https://user-images.githubusercontent.com/101978292/217398272-57aa1bbd-fb9f-4329-b05b-b125b5ef9f03.jpg)


4. Make sure Jenkins is up and running: 
```
sudo systemctl status jenkins
```

![2](https://user-images.githubusercontent.com/101978292/217398295-1da2b8e5-ed3d-4548-9ed9-590fe0202cd5.jpg)


5. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group
6. Next, setup Jenkins. From your browser access `http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080` You will be prompted to provide a default admin password

![3](https://user-images.githubusercontent.com/101978292/217398321-9491c95d-ef13-4f82-b9b3-1418946836c1.jpg)


7. Retrieve the password from your Jenkins server: 
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

```

![4](https://user-images.githubusercontent.com/101978292/217398509-208ac4a1-3977-4092-bd1f-d24a2ee50ab5.jpg)

8. Copy the password from the server and paste on Jenkins setup to unlock Jenkins.
9. Next, you will be prompted to install plugins – **choose install suggested plugins**


![5](https://user-images.githubusercontent.com/101978292/217398696-e4f469a5-71a2-4296-bed5-bf0a8636e18f.jpg)
![6](https://user-images.githubusercontent.com/101978292/217398705-9584cdaa-baf5-4c98-b1bd-b61a322d3395.jpg)


10. Once plugins installation is done – create an admin user and you will get your Jenkins server address. **The installation is completed!**

![7](https://user-images.githubusercontent.com/101978292/217398739-039f1102-af55-48af-bb4f-a0568de3987d.jpg)


#### Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks
Here you configure a simple Jenkins job/project. This job will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings: 
```
Go to the 
DevopsToolsWebsite repository
Click on settings
Click on webhooks on the left panel
On the webhooks page under Payload URL enter: `http:// Jenkins server IP address/github-webhook`
Under content type select: application/json
Then add webhook
```
![8](https://user-images.githubusercontent.com/101978292/217398817-92b7de5d-9516-4773-8223-27868f9867a3.jpg)


2. Go to Jenkins web console, click **New Item** and create a **Freestyle project** and click OK
3. Connect your GitHub repository, copy the repository URL from the repository
4. In configuration of your Jenkins freestyle project under Source Code Management select **Git repository**, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![9](https://user-images.githubusercontent.com/101978292/217399139-b719a731-55f6-4340-8ec3-f4ba79957d2b.jpg)

![10](https://user-images.githubusercontent.com/101978292/217399209-47c65df8-2963-47a9-a0cc-89e54727ad36.jpg)


5. Save the configuration and let us try to run the build. For now we can only do it manually.
6. Click **Build Now** button, if you have configured everything correctly, the build will be successfull and you will see it under **#1**
7. Open the build and check in **Console Output** if it has run successfully.


![12](https://user-images.githubusercontent.com/101978292/217399432-7691320a-902d-4302-96a0-616596a9acd5.jpg)




This build does not produce anything and it runs only when it is triggered manually. Let us fix it.
8. Click **Configure** your job/project and add and save these two configurations:
``` 
Under **Build triggers** select: Github trigger for GITScm polling
Under **Post Build Actions** select Archieve the artifacts and enter `**` in the text box.
```

![11](https://user-images.githubusercontent.com/101978292/217399770-8ab0064b-716f-4cab-bb0a-a34ebbecfd20.jpg)


9. Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the main branch.
10. You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.
11. We have successfully configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub).

![13](https://user-images.githubusercontent.com/101978292/217399835-caa9ee2f-03ed-4492-837e-011dc439d286.jpg)

12. By default, the artifacts are stored on Jenkins server locally: `ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/`

#### Step 3 – Configure Jenkins to copy files to NFS server via SSH
1. On your NFS Server, create a password for the user **ec2-user** 
2. switch to root and create the password, follow the prompt and add your password.

```
sudo su
passwd ec2-user
```

![14](https://user-images.githubusercontent.com/101978292/217400418-50dafccd-5aa3-46f8-b004-e303725a3548.jpg)


3. Now authenticate your password by opening the `sshd_config` file and changing the `PaaswordAuthentication no , To yes`

```
sudo vi /etc/ssh/sshd_config
```
![15](https://user-images.githubusercontent.com/101978292/217400438-8d696cae-0161-4632-82bf-e5df818d29cc.jpg)


4. Change the mode and ownership of `mnt and apps directory` on the NFS server from root to the ec2-user with the following:

```
ll /mnt
sudo chown -R nobody:nobody /mnt
sudo chmod -R 777 /mnt
sudo chown -R nobody:nobody /mnt/apps
sudo chmod -R 777 /mnt/apps

```

5. Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory. We need a plugin called
**Publish over SSh**
6. Install "Publish Over SSH" plugin.
7. Navigate to the dashboard select **Manage Jenkins** and choose **Manage Plugins** menu item.
8. On **Available** tab search for **Publish Over SSH** plugin and install it


![pix14](https://user-images.githubusercontent.com/74002629/184093437-ab971150-bb70-4393-a201-dc17617dd776.PNG)

9. Configure the job/project to copy artifacts over to NFS server.
10. On main dashboard select **Manage Jenkins** and choose **Configure System** menu item.
11. Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

```

Name- NFS
Hostname – can be private IP address of your NFS server
Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server
Passphrase (the password of the server)

```
![17](https://user-images.githubusercontent.com/101978292/217400934-da62e7c4-977f-48ab-8e86-2ac74f6acd1e.jpg)


12. Test the configuration and make sure the connection returns **Success** Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

![18](https://user-images.githubusercontent.com/101978292/217401048-80806dce-8415-408f-866d-71166ad1a105.jpg)


14. Save the configuration and open your Jenkins job/project configuration page and add another one Post-build Action: **Set build actionds over SSH**
15. Configure it to send all files produced by the build into our previously define remote directory. In our case we want to copy all files and directories – so we use **

![19](https://user-images.githubusercontent.com/101978292/217401532-e5eab0ed-2500-4f90-b4a0-ac5a229692a2.jpg)


15. Save this configuration and go ahead, change something in **README.MD** file the GitHub Tooling repository.
16. Webhook will trigger a new job and in the "Console Output" of the job you will find something like this:
```
SSH: Transferred 25 file(s)
Finished: SUCCESS
```

![20](https://user-images.githubusercontent.com/101978292/217401581-54ef5f90-9fab-4a95-9b78-45c3c42a9f3a.jpg)


17. To make sure that the files in /mnt/apps have been updated – to your NFS server and check README.MD file: `cat /mnt/apps/README.md`
18. If you see the changes you had previously made in your GitHub – the job works as expected.



#### Issues
1. I kept getting when i test the configuration of Publish Over SSH
`jenkins.plugins.publish_over.BapPublisherException: Failed to connect and initialize SSH connection. Message: [Failed to connect session for config [NFS].` 
2. Create and authenticate a password for the server and use the password in the passphrase section and do not use ssh_keygen.

```
sudo su
passwd ec2-user

```

 Now authenticate your password by opening the `sshd_config` file and changing the `PaaswordAuthentication no , To yes`

```
sudo vi /etc/ssh/sshd_config
```

1. After step 11, I got a "Permission denied" error which indicated that by build was not successful
2. I fixed the issue by changing mode and ownership on the NFS server with the following:
```
ll /mnt
sudo chown -R nobody:nobody /mnt
sudo chmod -R 777 /mnt
```
