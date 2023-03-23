Step 1- Install Jenkins server
Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
Install JDK (Java development kit) since Jenkins is a Java-based application:
```yml
sudo apt update
sudo apt install default-jdk-headless
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

 
Cd into ansible-config-mgt then
switch to the branch ansible-config-pri-14
`git checkout ansible-config-pri-14`

follow the instructions on the READ.ME file which shows the services to install ansible
