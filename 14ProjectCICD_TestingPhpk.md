# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP

## END-TO-END IMPLEMENTATION OF CI/CD PIPELINE FOR A PHP BASED APPLICATION

![1](https://user-images.githubusercontent.com/74002629/192497609-7c4c4583-ecfd-4f68-9b0a-b3bb3df7d91a.PNG)


##### Prerequsites
1. Servers: I will be making use of AWS virtual machines for this and will require 6 servers for the project which includes:
- nginx server: This would act as the reverse proxy server to our site and tool. 
- Jenkins server: To be used to implement your CI/CD workflows or pipelines. Select a t2.medium at least, Ubuntu 20.04 and Security group should be open to port 8080
- SonarQube server: To be used for Code quality analysis. Select a t2.medium at least, Ubuntu 20.04 and Security group should be open to port 9000
- Artifactory server: To be used as the binary repository where the outcome of your build process is stored. Select a t2.medium at least and Security group should be open to port 8081
- Database server: To server as the databse server for the Todo application
- Todo webserver: To host the Todo web application.
2. Secuirty groups: For the purposes of this project, you can have one security group that is open to all traffic. This should however not be attempted in a real DevOps enviroment.
3. Your Ansible inventory should look like this  
```
├── ci
├── dev
├── pentest
├── pre-prod
├── prod
├── sit
└── uat
```
focus will be mainly on the CI, Dev and Pentest enviroments 

4. Ansible roles for the CI environment. In addition to the previous Ansible roles from project 13, in your ansibile-config-mgt repo add 2 more roles: [Sonarqube](https://www.sonarqube.org/) and [Artifactory](https://jfrog.com/artifactory/).

#### Phase 1
##### Prepare your Jenkins server
1. Set up SSH-agent: 
```
eval `ssh-agent -s`
ssh-add <path-to-private-key>
```
2. Connect to your Jenkins instance on VScode.
3. Install the following packages and dependencies on the server:
- Install git : `sudo apt install git`
- Clone dwn the Asible-config-mgt repository: `git clone https://github.com/cynthia-okoduwa/ansible-config-mgt.git`
- Install Jenkins and its dependencies. Steps to install Jenkins can be found [here](https://www.jenkins.io/doc/book/installing/)
4. Configure Ansible For Jenkins Deployment. See [Project 9](https://github.com/cynthia-okoduwa/DevOps-projects/blob/main/Project9.md) for the initial setup of Jenkins. Here I will be comfiguring Jenkins to run Ansible commands in Jenkins UI.
- Navigate to Jenkins URL: `<Jenkins-server-public-IP>:8080`
- In the Jenkins dashboard, click on Manage Jenkins -> Manage plugins and search for Blue Ocean plugin. Install and open Blue Ocean plugin.
![pix1](https://user-images.githubusercontent.com/74002629/192139875-9d78fb62-afd5-4999-b8a8-0c40e5acca34.PNG)

Part 2 – Configuring Ansible For Jenkins Deployment

In previous projects, you have been launching Ansible commands manually from a CLI. Now, with Jenkins, we will start running Ansible from Jenkins UI.

Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
Install JDK (Java development kit) since Jenkins is a Java-based application:

Install JDK (since Jenkins is a Java-based application)
    
```yml
sudo apt update
sudo apt install default-jdk-headless -y
```
```yml
# open the bash profile 
vi .bash_profile 
```
```yml
# paste the below in the bash profile
export JAVA_HOME=$(dirname $(dirname $(readlink $(readlink $(which javac)))))
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar

# reload the bash profile
source ~/.bash_profile
```
Install Jenkins: The installations usually change, you can use this link to cross check:


Install Jenkins
```
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
        /etc/apt/sources.list.d/jenkins.list'
    sudo apt update -y
    sudo apt-get install jenkins -y

```

 ```
 https://www.jenkins.io/doc/book/installing/linux/
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
sudo systemctl status jenkins
```

Make sure Jenkins is up and running

    sudo systemctl status jenkins

![1 d](https://user-images.githubusercontent.com/10243139/137590173-c640787a-088d-47e6-a3f3-0e4ef9abe94f.png)

Perform initial Jenkins setup.

From your browser access, navigate to:

    http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

You will be prompted to provide a default admin password

Retrieve the default admin password from your server:

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Navigate to Jenkins URL

    http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

![1 g](https://user-images.githubusercontent.com/10243139/137590326-f4d9afc5-a050-4e71-aea9-52ebe4d27f3b.png)

- Install & Open Blue Ocean Jenkins Plugin

![1 h](https://user-images.githubusercontent.com/10243139/137590374-f85d79c7-d6a1-4cc5-8fbb-103425f49213.png)

- Create a new pipeline

- Select GitHub

<img width="1235" alt="Jenkins-Select-Github" src="https://user-images.githubusercontent.com/10243139/137590465-243ba7a0-d2e1-4218-92fb-e0c4aefcade7.png">

- Connect Jenkins with GitHub

<img width="1261" alt="Jenkins-Create-Access-Token-To-Github" src="https://user-images.githubusercontent.com/10243139/137590482-6bb60d31-d792-4873-ab8c-cf3b5a19823b.png">

- Login to GitHub & Generate an Access Token

![Jenkins-Github-Access-Token](https://user-images.githubusercontent.com/10243139/137590510-14f762c9-4a58-4551-a8cb-441a7cc05fe1.png)
<img width="1019" alt="Jenkins-Github-Generate-Token" src="https://user-images.githubusercontent.com/10243139/137590551-e43b78ed-e54c-4e1e-a794-f1368a143677.png">

- Copy Access Token

<img width="1185" alt="Jenkins-Copy-Token" src="https://user-images.githubusercontent.com/10243139/137590581-5ea60eed-04f7-4358-963c-af1fe30af15f.png">

- Paste the token and connect

<img width="1260" alt="JEnkins-Paste-Token-And-Connect" src="https://user-images.githubusercontent.com/10243139/137590598-e30f3f6b-9ab0-4eea-82ee-5e56d3335458.png">

- Create a new Pipeline

![1 o](https://user-images.githubusercontent.com/10243139/137590387-67732d4f-ea2e-496c-8c01-bf7c8f6eac82.png)

- Here is our newly created pipeline. It takes the name of your GitHub repository.

![1 p](https://user-images.githubusercontent.com/10243139/137590405-d32ae185-4912-49d2-8086-4a34031277a1.png)

Let us create our Jenkinsfile

Inside the Ansible project, create a new directory `deploy` and start a new file `Jenkinsfile` inside the directory.

![pix7](https://user-images.githubusercontent.com/74002629/192139899-94eb40be-fd85-467e-b72e-e5a2d699ed3b.PNG)

- Add the code snippet below to start building the test Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage


    pipeline {
        agent any

      stages {
        stage('Build') {
          steps {
            script {
              sh 'echo "Building Stage"'
            }
          }
        }
        }
    }

![1 r](https://user-images.githubusercontent.com/10243139/137590809-1c242ba4-d5b9-4d79-9d0e-304de99fc580.png)

Now go back into the Ansible pipeline in Jenkins, and select configure

![Jenkins-Select-Configure](https://user-images.githubusercontent.com/10243139/137590894-c221e266-82d2-4ff1-b5fa-10bf5a675040.png)

Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile

<img width="1434" alt="Jenkinsfile-Location" src="https://user-images.githubusercontent.com/10243139/137590935-93c44d00-088f-4d9e-978b-dbea5f087e6e.png">

Back to the pipeline again, this time click "Build now"

<img width="810" alt="Jenkins-Build-Now" src="https://user-images.githubusercontent.com/10243139/137591069-02b380a0-26f3-4715-a624-8d3e0e7630c4.png">

This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.


To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.

- Click on Blue Ocean
- Select your project
- Click on the play button against the branch

![1 w](https://user-images.githubusercontent.com/10243139/137591155-0d462f72-17bd-454d-8a12-ac70e79c931a.png)

### Add-On Bonus: Executing Multi-Branch Pipeline 

Notice that this pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

Let us see this in action.

Create a new git branch and name it feature/jenkinspipeline-stages

    Git checkout -b feature/jenkinspipeline-stages

Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub.

       pipeline {
        agent any

      stages {
        stage('Build') {
          steps {
            script {
              sh 'echo "Building Stage"'
            }
          }
        }

        stage('Test') {
          steps {
            script {
              sh 'echo "Testing Stage"'
            }
          }
        }
        }
    }

![1 x b](https://user-images.githubusercontent.com/10243139/137591317-5859dcd0-095a-4aec-a7ea-e7e0aa248a69.png)

To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository.

- Click on the "Administration" button
- Navigate to the Ansible project and click on "Scan repository now"

![Jenkins-Scan-Repository-Now](https://user-images.githubusercontent.com/10243139/137591377-ab4434dc-b5c1-4066-afc2-c4f9cfe9706f.png)

Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too.

![1 x d](https://user-images.githubusercontent.com/10243139/137591394-620ff58d-9106-49bf-8a77-063782f24f59.png)

In Blue Ocean, you can now see how the Jenkinsfile has caused a new step in the pipeline launch build for the new branch.

![1 x e](https://user-images.githubusercontent.com/10243139/137591408-a6cca3b7-f46f-4bac-89ca-8e1d71601be8.png)


## A QUICK TASK FOR YOU!

- Create a pull request to merge the latest code into the main branch
- After merging the PR, go back into your terminal and switch into the main branch.
- Pull the latest change.
- Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
    - Package 
    - Deploy 
    - Clean up
- Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch
- Eventually, your main branch should have a successful pipeline like this in blue ocean

## SOLUTION

Create a pull request to merge the latest code into the main branch

    git checkout main
    git merge feature/jenkinspipeline-stages
    
![1 y a](https://user-images.githubusercontent.com/10243139/137591523-1dc81cc6-6f6f-4651-bef9-1f19eb34a072.png)

Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
   
- Package 
- Deploy 
- Clean up

        Git checkout -b feature/jenkinspipeline-morestages

Add the code below into the Jenkinsfile

       pipeline {
        agent any

      stages {
        stage('Build') {
          steps {
            script {
              sh 'echo "Building Stage"'
            }
          }
        }

        stage('Test') {
          steps {
            script {
              sh 'echo "Testing Stage"'
            }
          }
        }


        stage('Package') {
          steps {
            script {
              sh 'echo "Packaging Stage"'
            }
          }
        }

        stage('Deploy') {
          steps {
            script {
              sh 'echo "Deploying Stage"'
            }
          }
        }

        stage('Clean up') {
          steps {
            script {
              sh 'echo "Cleaning up Stage"'
            }
          }
        }
        }

    }

![1 y b](https://user-images.githubusercontent.com/10243139/137591587-f5989669-c122-4b1f-965d-066d08fab069.png)

Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch

![1 y c](https://user-images.githubusercontent.com/10243139/137591669-66bef0b2-85ea-4718-8179-d38105b19a66.png)

Eventually, your main branch should have a successful pipeline like this in blue ocean

![1 y d](https://user-images.githubusercontent.com/10243139/137591678-524745c8-3b93-4ad0-bfff-941251e22404.png)

### Phase 2
#### Part 2 – Running Ansible Playbook from Jenkins

Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

1. Install Ansible on Jenkins
1. Install Ansible on Jenkins your ubuntu VM. Follow the steps in this link to insall [Ansible](https://www.cyberciti.biz/faq/how-to-install-and-configure-latest-version-of-ansible-on-ubuntu-linux/)


    sudo apt install Ansible -y

![2 a](https://user-images.githubusercontent.com/10243139/137591976-81bbc93d-6b23-4f40-b6ad-2a8b6b8cd711.png)


2. Install Ansible plugin in Jenkins UI

follow the instructions on the READ.ME file which shows the services to install ansible
- Install Ansible plugin in Jenkins UI

3. Install git and clone the ansible repo
```
sudo apt install git -y

git clone https://github.com/Rietta1/ansible-config-mgt.git

```

*Open the folder ansible-config using the side of the vscode and the prompt which will show at the top, to enable only ansible folder to show*
 
Cd into ansible-config-mgt then
switch to the branch ansible-config-pri-14
`git checkout ansible-config-pri-14`

4. Create Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully) Note: Ensure that Ansible runs against the **Dev** environment successfully. 

```
pipeline {
  agent any

  environment {
      ANSIBLE_CONFIG="${WORKSPACE}/deploy/ansible.cfg"
    }

  stages {
      stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

      stage('Checkout SCM') {
         steps{
            git branch: 'main', url: 'https://github.com/cynthia-okoduwa/ansible-config-mgt.git'
         }
       }

      stage('Prepare Ansible For Execution') {
        steps {
          sh 'echo ${WORKSPACE}' 
          sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'  
        }
     }

      stage('Run Ansible playbook') {
        steps {
           ansiblePlaybook become: true, credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory/dev, playbook: 'playbooks/site.yml'
         }
      }

      stage('Clean Workspace after build') {
        steps{
          cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
        }
      }
   }

}
```

**Some possible errors to watch out for:**
- Ensure that the git module in Jenkinsfile is checking out SCM to **main** branch instead of **master** (GitHub has discontinued the use of Master)
- Jenkins needs to export the ANSIBLE_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set. Enter this into the ancible.cfg file:

```

[defaults]
timeout = 160
callback_whitelist = profile_tasks
log_path=~/ansible.log
host_key_checking = False
gathering = smart
ansible_python_interpreter=/usr/bin/python3
allow_world_readable_tmpfiles=true


[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=30m -o ControlPath=/tmp/ansible-ssh-%h-%p-%r -o ServerAliveInterval=60 -o ServerAliveCountMax=60 -o ForwardAgent=yes

```
- Remember that ansible.cfg must be exported to environment variable so that Ansible knows where to find **Roles**. But because you will possibly run Jenkins from different git branches, the location of Ansible roles will change. Therefore, you must handle this dynamically. You can use Linux Stream Editor sed to update the section roles_path each time there is an execution. You may not have this issue if you run only from the main branch.

- If you push new changes to Git so that Jenkins failure can be fixed. You might observe that your change may sometimes have no effect. Even though your change is the actual fix required. This can be because Jenkins did not download the latest code from GitHub. Ensure that you start the Jenkinsfile with a clean up step to always delete the previous workspace before running a new one. Sometimes you might need to login to the Jenkins Linux server to verify the files in the workspace to confirm that what you are actually expecting is there. Otherwise, you can spend hours trying to figure out why Jenkins is still failing, when you have pushed up possible changes to fix the error.

- Another possible reason for Jenkins failure sometimes, is because you have indicated in the Jenkinsfile to check out the main git branch, and you are running a pipeline from another branch. So, always verify by logging onto the Jenkins box to check the workspace, and run git branch command to confirm that the branch you are expecting is there.
4. Parameterizing Jenkinsfile For Ansible Deployment. So far we have been deploying to dev environment, what if we need to deploy to other environments? We will use parameterization so that at the point of execution, the appropriate values are applied. To parameterize Jenkinsfile For Ansible Deployment, Update CI inventory with new servers
```
[tooling]
<SIT-Tooling-Web-Server-Private-IP-Address>

[todo]
<SIT-Todo-Web-Server-Private-IP-Address>

[nginx]
<SIT-Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<SIT-DB-Server-Private-IP-Address>
```
5. Update Jenkinsfile to introduce parameterization. Below is just one parameter. It has a default value in case if no value is specified at execution. It also has a description so that everyone is aware of its purpose.

```
pipeline {
    agent any

    parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
```

### DEPLOYING A CI/CD PIPELINE FOR TODO APPLICATION

Our goal here is to deploy the Todo application onto servers directly from **Artifactory** rather than from **git**. 


1. Next, fork the Todo repository below into your GitHub account
`https://github.com/darey-devops/php-todo.git`

2. On you Jenkins server, install PHP, its dependencies and Composer tool checkout this [link](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04) and this [link](https://getcomposer.org/download/)
    ```
    sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}

    ```

- Install composer 

```
 curl -sS https://getcomposer.org/installer | php 
 sudo mv composer.phar /usr/bin/composer

composer update --lock

apt-get install aptitude

sudo apt-get install aptitude

 php --version

 sudo aptitude install php8.1-curl

 stopped at composer installation

```
- Verify Composer is installed or not `composer --version`

3. In Jenkins UI install the following Jenkins plugins: 
**Plot plugin** to display tests reports, and code coverage information.
**Artifactory plugin** will be used to easily upload code artifacts into an Artifactory server.
4. Updated Ansible with an Artifactory role, use this guide to create an Ansible role for Artifactory (ignore the Nginx part). [Configure Artifactory on Ubuntu 20.04](https://www.howtoforge.com/tutorial/ubuntu-jfrog/) watch this [video](https://www.youtube.com/watch?v=tEyAEGwXQGk)

*add the private ippaddress of the articatory server  to the ci file in inventory folder*

sudo chmod -R 777 roles/artifactory

```
wget https://releases.jfrog.io/artifactory/artifactory-rpms/artifactory-rpms.repo -O jfrog-artifactory-rpms.repo;

sudo mv jfrog-artifactory-rpms.repo /etc/yum.repos.d/;

sudo yum update && sudo yum install jfrog-artifactory-oss

sudo systemctl enable  artifactory.service

sudo systemctl start artifactory.service

sudo systemctl status artifactory.service
```

5. Now, open your web browser and type the URL https://. You will be redirected to the Jfrog Atrifactory page. Enter default username and password: admin/password. Once in create username and password and create your new repository. (Take note of the reopsitory name)
![pix23](https://user-images.githubusercontent.com/74002629/192488480-5562cbb1-d39e-4dfe-83e1-ced7b7113786.PNG)


6. In Jenkins UI configure Artifactory
- Go to Dashboard
- System configuration
- Configure the server ID, URL and Credentials, run Test Connection.

```
instance id = artifactory
host =
jfrog = admin
password = Password@
```

### Phase 2 – Integrate Artifactory repository with Jenkins



2. Go to mysql on the roles section `mysql/default/main.yml` and uncomment the second database setup and add the jenkins private ipaddr, check that the dbservers ipaddr is correct, then commit and build with dev in jenkins

1. In VScode create a new Jenkinsfile in the Todo repository
2. Using Blue Ocean, create a multibranch Jenkins pipeline
3. Install my sql client: `sudo apt install mysql -y`
4. Login into the DB-server(mysql server) and set the the bind address to 0.0.0.0: `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`
![pix27](https://user-images.githubusercontent.com/74002629/192424564-bf94bce3-dd50-4753-88fb-35f297e8f15f.PNG)

5. Restart the my sql- server: `sudo systemctl restart mysql`

6. Update the database connectivity requirements in the file .env.sample (DB_Host is the Private IP of the DB-server)
```
DB_HOST=172.31.62.127
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=sePret^i
DB_CONNECTION=mysql 
DB_PORT=3306
```

7.  On the database server, create database and user
```
Create database homestead;
CREATE USER 'homestead'@'%' IDENTIFIED BY 'sePret^i';
GRANT ALL PRIVILEGES ON * . * TO 'homestead'@'%';
```

8. Log into mysql from VScode: mysql -h 172.31.87.194 -u homestead -p (at the promot enter pasword)
![pix26](https://user-images.githubusercontent.com/74002629/192424380-386ee0af-aae6-4b6c-a8f5-31b8a35b63a7.PNG)

9. Update Jenkinsfile with proper pipeline configuration. In the Checkout SCM stage ensure you specify the branch as main and change the git repository to yours.
```
pipeline {
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

    stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/Rieeta1/php-todo.git'
      }
    }

    stage('Prepare Dependencies') {
      steps {
             sh 'mv .env.sample .env'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}
```
Notice the Prepare Dependencies section
- The required file by PHP is **.env** so we are renaming **.env.sample** to **.env**

## *i stopped the setup here to continue later, composer was not working, 2hrs40mins on video*
- Composer is used by PHP to install all the dependent libraries used by the application. php artisan uses the .env file to setup the required database objects – (After the successful run of this step, login to the database, run show tables and you will see the tables being created for you)

10. Commit to main repo and run the build on Jenkins
![pix31](https://user-images.githubusercontent.com/74002629/192425128-fdddb299-9eac-4b44-b58e-87751f97c552.PNG)

11. Update the Jenkinsfile to include Unit tests step
```
    stage('Execute Unit Tests') {
      steps {
             sh './vendor/bin/phpunit'
      } 
```
### Phase 3 – Code Quality Analysis
For PHP the most commonly tool used for code quality analysis is **phploc**. [Read the article here for more](https://matthiasnoback.nl/2019/09/using-phploc-for-quick-code-quality-estimation-part-1/)
The data produced by phploc can be ploted onto graphs in Jenkins.
1. Install phploc: 
```
sudo apt update -y && sudo apt upgrade -y

sudo apt install phpunit -y

phpunit --version

```
2. Add the code analysis step in Jenkinsfile. The output of the data will be saved in **build/logs/phploc.csv** file.
```
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}
```
3. Plot the data using Plot Jenkins plugin.
This plugin provides generic plotting (or graphing) capabilities in Jenkins. It will plot one or more single values variations across builds in one or more plots. Plots for a particular job (or project) are configured in the job configuration screen, where each field has additional help information. Each plot can have one or more lines (called data series). After each build completes the plots’ data series latest values are pulled from the CSV file generated by phploc.
```
    stage('Plot Code Coverage Report') {
      steps {

            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Lines of Code (LOC),Comment Lines of Code (CLOC),Non-Comment Lines of Code (NCLOC),Logical Lines of Code (LLOC)                          ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'A - Lines of code', yaxis: 'Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Directories,Files,Namespaces', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'B - Structures Containers', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Average Class Length (LLOC),Average Method Length (LLOC),Average Function Length (LLOC)', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'C - Average Length', yaxis: 'Average Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Cyclomatic Complexity / Lines of Code,Cyclomatic Complexity / Number of Methods ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'D - Relative Cyclomatic Complexity', yaxis: 'Cyclomatic Complexity by Structure'      
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Classes,Abstract Classes,Concrete Classes', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'E - Types of Classes', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Methods,Non-Static Methods,Static Methods,Public Methods,Non-Public Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'F - Types of Methods', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Constants,Global Constants,Class Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'G - Types of Constants', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Test Classes,Test Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'I - Testing', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Logical Lines of Code (LLOC),Classes Length (LLOC),Functions Length (LLOC),LLOC outside functions or classes ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'AB - Code Structure by Logical Lines of Code', yaxis: 'Logical Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Functions,Named Functions,Anonymous Functions', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'H - Types of Functions', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Interfaces,Traits,Classes,Methods,Functions,Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'BB - Structure Objects', yaxis: 'Count'

      }
    }
```
You should now see a Plot menu item on the left menu. Click on it to see the charts.
![pix33](https://user-images.githubusercontent.com/74002629/192425751-e7fc5dc2-8362-438f-8607-0065c0cc0729.PNG)
![pix34](https://user-images.githubusercontent.com/74002629/192425753-5ffc7023-79b1-40db-a488-d838d59665b6.PNG)

3. Bundle the application code into an artifact (archived package) and upload to Artifactory
- Install Zip: Sudo apt install zip -y
```
stage ('Package Artifact') {
    steps {
            sh 'zip -qr php-todo.zip ${WORKSPACE}/*'
     }
    }
```
4. Publish the resulted artifact into Artifactory making sure ti specify the target as the name of the artifactory repository you created earlier
```
stage ('Upload Artifact to Artifactory') {
          steps {
            script { 
                 def server = Artifactory.server 'artifactory-server'                 
                 def uploadSpec = """{
                    "files": [
                      {
                       "pattern": "php-todo.zip",
                       "target": "PBL/php-todo",
                       "props": "type=zip;status=ready"

                       }
                    ]
                 }""" 

                 server.upload spec: uploadSpec
               }
            }

        }
```
5. Push and run your build in Jenkins
![pix36](https://user-images.githubusercontent.com/74002629/192427223-4c55129d-c84f-425d-babd-81b6ce4b4991.PNG)

6.  Log in to your repository in Jfrog artifactory to see the packaged artifact.
![pix37](https://user-images.githubusercontent.com/74002629/192427240-d2764381-44dd-46a2-98a9-4af3e29a3147.PNG)

7. Deploy the application to the dev environment by launching Ansible pipeline. Ensure you update your inventory/dev with the Private IP of your TODO-server and your site.yml file is updated with todo play.
```
stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-project/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
```
- This particular stage, once it completes the upload to arifactory, it would trigger a call to the your ansible-config-mgt/static-assignments/deployment.yml file and execute the instructions there. Ensure you update the "Download the artifact" instruction with your artifactory url_username and url_password for your artifactory repo. 
![pix39](https://user-images.githubusercontent.com/74002629/192427578-055a05ed-233d-4183-b037-38c356870a58.PNG)

8. Next we want to ensure that the code being deployed has the quality that meets corporate and customer requirements. We have implemented Unit Tests and Code Coverage Analysis with **phpunit** and **phploc**, we still need to implement [Quality Gate](https://docs.sonarqube.org/latest/user-guide/quality-gates/) to ensure that ONLY code with the required code coverage, and other quality standards make it through to the environments. To achieve this, we need to configure [SonarQube](https://docs.sonarqube.org/latest/) – An open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.
9. Install SonarQube on Ubuntu 20.04 With PostgreSQL as Backend Database, Create Sonarqube roles. You can do this manaually or write a script with the directions below or go to [Ansible Galaxy](https://galaxy.ansible.com/search?deprecated=false&keywords=&order_by=-relevance) to find a sonarqube role
![4](https://user-images.githubusercontent.com/74002629/192433173-4130fcd1-d6df-45cb-a6e0-6e7fdf3b6985.PNG)
![5](https://user-images.githubusercontent.com/74002629/192433188-64afa5fe-e1c5-4942-bffb-f1619de83756.PNG)
![6](https://user-images.githubusercontent.com/74002629/192433214-c7665fe5-8dfc-4bec-adb8-636aa4f30425.PNG)
10. Ensure your Sonarqube server is listed on your inventory/ci file.
11. Update your site.yml with sonarqube play instruction.
12. Next copy your paste your public IP for your sonarqube server in your browser to access the SonarQube UI: <sonar-public-IP:9000/sonar>
13. Login with Username and Password as admin/admin
![pix42](https://user-images.githubusercontent.com/74002629/192487591-59a01fb3-1ee8-45ac-9b04-a636ec821c03.PNG)

14. Confiure Sonar in Jenkins
- install **SonarQube Scanner plugin**
- Navigate to configure system in Jenkins. Add SonarQube server: `Manage Jenkins > Configure System`
- To generate authentication token in SonarQube to to: `User > My Account > Security > Generate Tokens`
![pix43](https://user-images.githubusercontent.com/74002629/192487160-804a2fe7-92c3-4f5c-aa95-71d7f6dee1e9.PNG)

- Configure Quality Gate Jenkins Webhook in SonarQube – The URL should point to your Jenkins server http://{JENKINS_HOST}/sonarqube-webhook/ Go to:`Administration > Configuration > Webhooks > Create`
- Setup SonarQube scanner from Jenkins – Global Tool Configuration. Go to: `Manage Jenkins > Global Tool Configuration`
15. Update Jenkins Pipeline to include SonarQube scanning and Quality Gate. Making sure to place it before the "package artifact stage" Below is the snippet for a Quality Gate stage in Jenkinsfile.
```
    stage('SonarQube Quality Gate') {
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
            }

        }
    }
```
NOTE: The above step will fail because we have not updated **sonar-scanner.properties**
16. Configure sonar-scanner.properties – From the step above, Jenkins will install the scanner tool on the Linux server. You will need to go into the tools directory on the server to configure the properties file in which SonarQube will require to function during pipeline execution.
`cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/conf/`
17. Open sonar-scanner.properties file: `sudo vi sonar-scanner.properties`
18. Add configuration related to php-todo project
```
sonar.host.url=http://<SonarQube-Server-IP-address>:9000
sonar.projectKey=php-todo
#----- Default source code encoding
sonar.sourceEncoding=UTF-8
sonar.php.exclusions=**/vendor/**
sonar.php.coverage.reportPaths=build/logs/clover.xml
sonar.php.tests.reportPath=build/logs/junit.xml 
```
### End-to-End Pipeline Overview
Congratulations on the job so far. If everything has worked out for you so far, you should have a view like below:
![pix44](https://user-images.githubusercontent.com/74002629/192485051-3c1a1ba2-c204-4c01-b44f-af3a1badf1bd.PNG)

But we are not completely done yet. The quality gate we just included has no effect. Why? Well, because if you go to the SonarQube UI, you will realise that we just pushed a poor-quality code onto the development environment. Navigate to php-todo project in SonarQube, there are bugs, and there is 0.0% code coverage. (code coverage is a percentage of unit tests added by developers to test functions and objects in the code)

![pix45](https://user-images.githubusercontent.com/74002629/192485802-22d43337-ab3e-41bc-a1fa-71d3fbbdcc95.PNG)

If you click on php-todo project for further analysis, you will see that there is 6 hours’ worth of technical debt, code smells and security issues in the code.
First, we will include a When condition to run Quality Gate whenever the running branch is either develop, hotfix, release, main, or master
when { branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"}
Then we add a timeout step to wait for SonarQube to complete analysis and successfully finish the pipeline only when code quality is acceptable.
    timeout(time: 1, unit: 'MINUTES') {
        waitForQualityGate abortPipeline: true
    }
The complete stage will now look like this:

    stage('SonarQube Quality Gate') {
      when { branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"}
        environment {
            scannerHome = tool 'SonarQubeScanner'
        }
        steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
            }
            timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
        }
    }
To test, create different branches and push to GitHub. You will realise that only branches other than develop, hotfix, release, main, or master will be able to deploy the code.

If everything goes well, you should be able to see something like this:

composer update --lock

apt-get install aptitude

sudo apt-get install aptitude

 php --version

 sudo aptitude install php8.1-curl

 stopped at composer installation