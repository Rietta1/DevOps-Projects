#  Linux Client/Server Architecture Using A MySQL RDS


###  Client-Server Architecture
Client-server architecture is a type of computer network architecture in which multiple client computers request and receive services or resources from a central server. The clients and servers communicate with each other using a set of protocols, such as HTTP or TCP/IP.

In this architecture, the client is responsible for providing a user interface and handling user input, while the server is responsible for managing and providing access to shared resources, such as databases, files, and services. The client and server can run on the same physical machine or on different machines connected over a network.

One of the main advantages of client-server architecture is its ability to separate the presentation and processing layers, making it easy to distribute the load between different machines and scale the system as needed. This allows for better performance and flexibility, as well as the ability to easily update or replace either the client or server components without affecting the other.

It is widely used in web applications, where a web browser acts as the client, and a web server handles the requests and provides resources.
![Step 1 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/0aeb1b0b-93f9-4096-b04f-662c1b0fd606/c7d52468-67b8-49e9-a75d-2f47daad0706.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


The client/server design, at its most basic, involves separating application processing into two or more conceptually different components.
The database is responsible for half of the client/server architecture. The database is the "server," and any program that accesses it is the "client." In many circumstances, the client and server run on different computers; in most cases, the client program is a user-friendly interface to the database.
![Step 2 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/67c10bfd-3c49-4e2a-b2f1-c38113e6cc3c/111086b0-6e24-48b4-8931-308aeb7bfb7d.jpeg?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


#### 1. [TASK – Implement a Client-Server Architecture using MySQL Database Management System (DBMS).](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)


Spin up and configure 2 new Linux EC2 Instances

```
Server A - mysql server
Server B - mysql client
```
![Step 4 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/f354c3b2-d9d3-44e0-9963-205fc87c638b/34b08445-2ea7-4053-beb5-5d85a40bdb54.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Update the latest updates on the server.
 ```
sudo apt update -y 
```


 On mysql server Linux Server, we will install the MySQL software.

```
  sudo apt install mysql-server -y
```

 Check the mysql version
```
 mysql --version
```

 Start the service
```
 sudo service mysql start
```

 Check the status 
```
 sudo service mysql status
```
![Step 9 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/86f56e03-a49c-47d0-b0f2-190942e2a4d7/95f49774-0575-42c1-8c24-e87f6245f719.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


#### 2. For mysql client to gain remote access to mysql server we need to create and database and a user on mysql server. To start with run the mysql script: 

```
  sudo mysql
```
![Step 10 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/26c92cd9-9129-4d52-bef4-4b5e82abf31c/f480bee7-c40e-425d-b712-0ada9180626a.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Check the plugin for the root user 
```
"SELECT user , host , plugin from mysql.user;" 
```
![Step 11 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/faf41102-d0bc-4ba3-bb09-c030b76bbd17/56b29f2c-ffac-4d1b-bd74-d9cb02b0a6e7.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Change the plugin by running these scripts

```
 "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';" 

```
![Step 12 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/b29dbe49-7655-440d-945c-a1fbbfc5458d/5940fa8f-117f-4b6c-82ab-488d036c9f04.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Check the new plugin:

```
 "SELECT user , host , plugin from mysql.user;" 

   "exit"

```
![Step 13 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/c15cd471-4684-4c2c-8169-8c89afee7546/a3748ce1-30dd-4c06-b58f-5b2ac2adae9d.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Now run the mysql security script:

```
 sudo mysql_secure_installation
```

 Follow the prompts and answer appropriately to finish the process.



![Step 14 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/c21107ad-84a5-4447-9fd9-392f6e8f3ee2/e634af2d-e113-49da-8f2e-7ec536800c9f.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Login to the root user 
```
 mysql -u root -p
```
input the password


![Step 15 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/c064b9be-66f2-4766-886a-8c1b301c1ac6/cf6c04b9-6911-4bd4-b21e-023398c77efc.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Check the plugin
```
  ` SELECT user , host , plugin from mysql.user; ` 
```
![Step 16 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/0af67c3c-2e0f-43c4-ad24-e0d3c5adec98/625269d1-9abd-4082-82eb-40af2fd39ac5.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 ```
 SHOW DATABASES; 
```
 to show the database table already in it

![Step 17 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/6cf8c740-d353-45fb-8100-f9990c2b2d1c/7249b9fb-6a15-419b-9213-d7dc1c67fccc.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Next, create the remote user with the following command:

```
 `CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`
```
![Step 18 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/8c6f8a12-78a2-4023-9b91-5ba0f1ca1079/9c72dd1b-c1dc-4a46-bedb-d4767c5190dd.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Create a database with:
```
CREATE DATABASE  loretta_db;
```
![Step 19 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/c5b4133e-eb1e-4fcf-9a95-681929eb9d8c/78cabb77-9e66-4db7-8b25-5a912c4e2d78.png?crop=focalpoint&fit=crop&fp-x=0.4927&fp-y=0.4317&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Then grant privileges to remote_user:

```
  GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;`

 . Finally, flush privileges: `FLUSH PRIVILEGES;
```
![Step 20 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/2263f3f8-0ae1-4384-b183-e6cd22abfa02/ff6cc7c7-9bac-4c0c-bf9b-376e057a3dea.png?crop=focalpoint&fit=crop&fp-x=0.4927&fp-y=0.4317&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Type "SHOW DATABASES" and exit mysql: `exit`
![Step 21 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/42e22bef-2c14-407a-ada1-9ebd432a1e04/ef6ee4e1-b286-47f9-9057-c50fa1058567.png?crop=focalpoint&fit=crop&fp-x=0.4811&fp-y=0.5105&fp-z=1.0379&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


#### 3. Having created the user and database, configure MySQL server to allow connections from remote hosts. Use the following command:
```
 sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```


 In the text editor, replace the old Bind-address from ‘127.0.0.1’ to ‘0.0.0.0’ then save and exit

Then restart the service
```
sudo systemctl restart mysql

```
![Step 23 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/9742584c-6142-4ced-9b5e-0074c330877a/6c4915d7-0d1c-464e-8dba-df83f7c7e323.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Next, we restart mysql with: sudo systemctl restart mysql


#### 4. On **mysql client** install MySQL Client software:

```
 sudo apt install mysql-client -y
```
![Step 25 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/bcace673-5aff-4799-b882-70b84b76ed88/db08108b-6436-464f-a26e-a2646c6a1552.png?crop=focalpoint&fit=crop&fp-x=0.4989&fp-y=0.5097&fp-z=1.0349&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 To get the IP address of your server

```
 IP addr
```
Copy it 
![Step 26 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/962e80d4-e0c1-4da1-8ffe-8ec3654906b4/56c7ed23-613c-4d7c-84be-12feaca8ad2a.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


 Edit Inbound rule on **mysql server** to allow access to **mysql client** traffic. MySQL server uses TCP port 3306 by default. Specify inbound traffic from the IP
of **mysql cient** for extra security.
![Step 27 screenshot](https://images.tango.us/workflows/5872cb84-1369-410a-b7c1-ae0d47cf115c/steps/a1690606-00c7-408b-afd6-297c849ad759/4b707979-b1f8-4e1c-a0b7-43d40b148e39.png?crop=focalpoint&fit=crop&fp-x=0.5000&fp-y=0.5000&w=1200&blend-align=bottom&blend-mode=normal&blend-x=800&blend64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n)


