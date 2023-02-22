## ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10

### IN THIS PROJECT WE WOULD WORK ON ANSIBLE CONFIGURATION MANAGEMENT WHERE WE WOULD AUTOMATE PROJECT 7 TO 10

By automating the majority of repetitive chores with Ansible Configuration Management, this project will increase your appreciation of DevOps tools while also boosting your comfort level with writing code in declarative languages like YAML.
<br></br>

![Screenshot 2023-02-22 010300](https://user-images.githubusercontent.com/101978292/220486930-f8fa9786-6aab-4f8e-81da-215b00e0bb0c.jpg)

<br></br>


### Install and configure Ansible client to act as a Jump Server/Bastion Host


An SSH jump server, also known as an SSH gateway or SSH bastion host, is a server that provides a secure way to access other servers in a remote network.

To use an SSH jump server, you first connect to the jump server using SSH. Once you are authenticated, you can then use the jump server to connect to other servers in the remote network. This is useful when the remote servers are not directly accessible from the public internet or when they are located behind a firewall.

Using an SSH jump server can improve security by limiting the number of direct connections to sensitive servers and by providing an additional layer of authentication and authorization. It also simplifies remote access to a network, as users only need to connect to a single server to access all the other servers in the network.

SSH jump servers are commonly used in enterprise environments, where they are used to provide secure remote access to servers and other resources. They are also used by system administrators and DevOps engineers to manage and deploy applications in remote networks.<br></br> 

### Task
1. Install and configure Ansible client to act as a Jump Server/Bastion Host
2. Create a simple Ansible playbook to automate servers configuration

### Requirments
Spin up 5 servers:
- 1 Ubuntu server for LB
- 4 Redhat server for WEB1&2, NFS and DB


#### Step 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE
1. Continuating from [project 9](https://github.com/Rietta1/DevOps-Projectss/blob/main/Project_CICD_ToolsWeb.md), update Name tag on your Jenkins EC2 Instance to **Jenkins-Ansible**. This server will be used to run playbooks.

2. In your GitHub account create a new repository and name it **ansible-config-mgt**.

3. In your **Jenkin-Ansible** server, install **Ansible**

```
sudo apt update
sudo apt install ansible
```

![1](https://user-images.githubusercontent.com/101978292/220487120-d4b01ad5-fde0-41fe-8703-a35b0577a710.jpg)


4. Check your Ansible version by running `ansible --version`

5. Configure Jenkins build job to save your repository content every time you change it. See [project 9](https://github.com/Rietta1/DevOps-Projectss/blob/main/Project_CICD_ToolsWeb.md) for detailed steps

  - Create a new Freestyle project ansible in Jenkins and point it to your **ansible-config-mgt** repository.

  - Configure Webhook in GitHub and set webhook to trigger ansible build.

  ![3](https://user-images.githubusercontent.com/101978292/220487202-2edbfecc-69d6-45f2-890c-f528580ee547.jpg)


  ![2](https://user-images.githubusercontent.com/101978292/220487151-7e2797c8-d27f-467a-abf4-c5bd4f94b116.jpg)


  - Configure a Post-build job to save all (**) files. 

  - Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder `ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`
  - 
  ![4](https://user-images.githubusercontent.com/101978292/220487256-92baa598-b780-43de-88e4-bb2e4ce3b07e.jpg)

  ![5](https://user-images.githubusercontent.com/101978292/220487327-7b647ab9-e0da-42a3-ab1f-b8a035850586.jpg)

  ![6](https://user-images.githubusercontent.com/101978292/220487375-1e0f7e6b-7dc5-4b8e-a451-fa8d84aed891.jpg)

    
#### Step 2 – Prepare your development environment using Visual Studio Code

1. Install Visual Studio Code (VSC)- an Integrated development environment (IDE) or Source-code Editor. You can get it [here](https://code.visualstudio.com/download)

2. Configure it to connect to your newly created GitHub repository.
- Go the extenstions and install **Remote Development**

- on the left down end you will see a green marking with >< , click on it it would open it will open a prompt at the top, click on **open ssh configuration**, then click on **Users/Hp/.ssh/config** *this shows where your ssh files are stored in your computer* Copy this into the page:

```
Host Jenkins-Ansible
  HostName 54.146.71.57
  User ubuntu
  IdentityFile ~/Downloads/devopsKP.pem
  ServerAliveInterval 60

```

![12](https://user-images.githubusercontent.com/101978292/220488545-3aa55a48-217a-4655-8c47-3bf3f287ef46.jpg)


- Save it, and click on the green corner again, click connect to host and click Jenkins-Ansible, follow the appropriate prompt and you are connected.


3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance: `git clone <ansible-config-mgt repo link>`


### Create a simple Ansible playbook to automate servers configuration

#### Step 3 - Begin Ansible development
1. In your **ansible-config-mgt** GitHub repository, create a new branch that will be used for development of a new feature.

2. Checkout the newly created feature branch to your local machine and start building your code and directory structure

3. Create a directory and name it **playbooks** – it will be used to store all your playbook files.

4. Create a directory and name it **inventory** – it will be used to keep your hosts organised.

5. Within the playbooks folder, create your first playbook, and name it **common.yml**

6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) **dev**, **staging**, **uat**, and **prod** respectively.

#### Step 4 – Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since the intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

1. Save below inventory structure in the inventory/dev **dev.yml** file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

2. Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Go into the directory your **.pem** is saved, in my own case its saved in the Downloads **cd Downloads** folder, Now you need to import your key into ssh-agent: 

```
eval `ssh-agent -s`
ssh-add <path-to-private-key>
```

3. Confirm the key has been added with this command, you should see the name of your key: `ssh-add -l`


![7 ssh agent](https://user-images.githubusercontent.com/101978292/220487511-a962a5e2-2d59-456b-8ecc-bca8ea024b19.jpg)


5. Now, ssh into your Jenkins-Ansible server using ssh-agent: `ssh -A ubuntu@public-ip`

6. Also notice, that your ubuntu user is ubuntu and user for RHEL-based servers is ec2-user.

7. Update your inventory/dev.yml file with this snippet of code:

```
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
```

![8](https://user-images.githubusercontent.com/101978292/220487577-f316cc24-e398-40f4-b090-6ddb85c322e2.jpg)


#### Step 5 – Create a Common Playbook

Now we give Ansible the instructions on what you needs to be performed on all servers listed in **inventory/dev**.

 In **common.yml** playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

1. Update your playbooks/common.yml file with following code:

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```

  ![9](https://user-images.githubusercontent.com/101978292/220487668-15c0769c-9d1d-4bd0-8e93-64d222624cc3.jpg)


2. This playbook is divided into two parts, each of them is intended to perform the same task: install **wireshark utility** (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses **root** user to perform this task and respective package manager: **yum** for RHEL 8 and **apt** for Ubuntu.

3. For a better understanding of Ansible playbooks – [watch this video](https://www.youtube.com/watch?v=ZAdJ7CdN7DY) and read [this article](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook) from Redhat.

4. Go to all the 5 servers spinned up, create a password and authenticate it for the users; That is create a password for the ec2-user for the Redhat Machines and for the Ubuntu for the Ubuntu server

```
sudo su
passwd ec2-user 
(book1)
```

- Authenticate the password by editting the configuration file **sshd_config** and change the **PasswordAthentication** to yes

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

#### Step 6 – Update GIT with the latest code

1. Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

2. Commit your code into GitHub: use git commands to **add**, **commit** and **push** your branch to GitHub.

```
git status
git add <selected files>
git commit -m "commit message"
```

4. Create a Pull request (PR)


5. Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to **/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/** directory on Jenkins-Ansible server.

  

#### Step 7 – Connect Jenkins-Ansible server to all the other servers

1. Ssh into the production server from the app server *it will request for the prodserver password*

```

ssh ec2-user@<Private-IP-Address>

```
2. After adding the password exit and go back to the jenkins-ansible server

3. Now To remove the password request. Generate keygen and copy to the production server, this enables easy access between production an application server *ensure you are signed in has ec2-user to create the ssh keygen and not as root* Follow the promote and keep pressing enter, do not add a paraphrase because we do not want to use password

```
ssh-keygen
```

4. Now copy the id into the server we went to have ssh access, which is to the production server

```
ssh-copy-id ec2-user@<Private-IP-Address>
```

*it will ask of the password to the server book1, no of users 1*

5.Now ssh into the server this will esterblished a perminet connection between our application server and production server now it wont request for a password again

```
ssh ec2-user@<Private-IP-Address>
```
*This ensures the jenkins-ansible has access to the other servers otherwise running the ansible playbook will fail*
<br></br>

#### Step 7 – Run Ansible test
1. Now, it is time to execute ansible-playbook command and verify if your playbook actually works: `cd ansible-config-mgt`


2. Run ansible-playbook command:

```
ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/playbooks/common.yml
```

 ![14](https://user-images.githubusercontent.com/101978292/220488218-c85de7a3-bfbf-430b-9eb2-9c7a0300b857.jpg)

![15](https://user-images.githubusercontent.com/101978292/220488188-720a7e73-aa0d-4df2-b01f-8d1e5392f5b6.jpg)


3. If your command ran successfully, you go to each of the servers and check if wireshark has been installed by running `which wireshark` or `wireshark --version`


4. Your updated with Ansible architecture now looks like this:

![Screenshot 2023-02-22 010300](https://user-images.githubusercontent.com/101978292/220488482-467cfac2-0e66-45d2-9091-553a69dc748f.jpg)


5. You have just automated your routine tasks by implementing your first Ansible project with Jenkins
