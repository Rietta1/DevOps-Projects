

#### Task
**Modify the Project 8 architecture by adding a Jenkins server and configuring a task to automatically publish source code updates from Git to an NFS server.**




#### Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks
Here you configure a simple Jenkins job/project. This job will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings: 
```
Go to the 
itern repository
Click on settings
Click on webhooks on the left panel
On the webhooks page under Payload URL enter: http:// Jenkins server IP address/github-webhook/
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



6. Click **Configure** your project1d and add save this configuration:

``` 
Under **Build triggers** select: Github trigger for GITScm polling

```



9. Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the main branch.
10. You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.


![13](https://user-images.githubusercontent.com/101978292/217399835-caa9ee2f-03ed-4492-837e-011dc439d286.jpg)

11. By default, the artifacts are stored on Jenkins server locally: `ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/`

#### Step 3 – Configure Jenkins to copy files to NFS server via SSH

**Publish over SSh**

9. Configure the job/project to copy artifacts over to APP server.
10. On main dashboard select **Manage Jenkins** and choose **Configure System** menu item.
11. Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

```

Name- appserver
Hostname – can be private IP address of your NFS server
Username – ec2-user 
Remote directory: . 
Passphrase (the password of the server)

```


12. Test the configuration and make sure the connection returns **Success** Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

![18](https://user-images.githubusercontent.com/101978292/217401048-80806dce-8415-408f-866d-71166ad1a105.jpg)




15. Save this configuration and go ahead, change something in **README.MD** file the GitHub Tooling repository.
16. Webhook will trigger a new job and in the "Console Output" of the job you will find something like this:
```
SSH: Transferred 25 file(s)
Finished: SUCCESS
```

![20](https://user-images.githubusercontent.com/101978292/217401581-54ef5f90-9fab-4a95-9b78-45c3c42a9f3a.jpg)



