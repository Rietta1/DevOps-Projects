## LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

In this project, We will deepen our knowledge of load balancers and become more adaptable in our knowledge of setting different types of LBs via this assignment. To automate certificate issuance, we will setup a Nginx Load Balancer solution and register our website with LetsEnrcypt Certificate Authority.

Load balancing is a good method for scaling out our program and improving its speed and reliability. Nginx, a popular web server software, may be configured as a simple yet effective load balancer to increase the availability and efficiency of our hosts' resources.

### Load balancing methods

** The following load balancing methods are supported in nginx:**

- Round-robin — requests to the application servers are distributed in a round-robin fashion,
- Least-connected — next request is assigned to the server with the least number of active connections,
- Ip-hash — a hash-function is used to determine what server should be selected for the next request (based on the client’s IP address).


A certificate is a security mechanism that creates an encrypted session between the browser and the Web server to protect the connection from MITM attacks. Cetrbot, a shell client recommended by LetsEncrypt, will be used in our project.

Nginx serves as a single point of access to a distributed web application that runs on numerous servers.

Our achitecture will look something like this:



![pix18](https://user-images.githubusercontent.com/74002629/184848922-0b777f13-bef5-4361-9a97-a3996c451f3e.PNG)


### Task 
This project consists of two parts:
1. Configure Nginx as a Load Balancer
2. Register a new domain name and configure secured connection using SSL/TLS certificates

#### Step 1 - CONFIGURE NGINX AS A LOAD BALANCER

1. Create an EC2 instance based on Ubuntu Server 20.04 LTS and name it **Nginx LB**, make sure to open TCP port 80 for HTTP connections and  
open TCP port 443 for secured HTTPS connections

2. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers, update the instance and Install Nginx:
```
sudo apt update
sudo apt install nginx
```


3. Disable the default link

```
sudo  unlink /etc/nginx/sites-enabled/default
```

4. Start Nginx at boot time**

```
 sudo systemctl enable nginx

 sudo systemctl start nginx

 sudo systemctl status nginx
```


![Screenshot 2023-02-02 021024](https://user-images.githubusercontent.com/101978292/216476419-7891207f-28db-4c63-85e3-89040c96d11f.jpg)


5. Let us now make use of above upstream and server block to create a load balancer for a domain. We need to create a configuration file using a text editor.

 Open the default nginx configuration file : 
 
 ```
 sudo vi /etc/nginx/sites-available/load_balancer.conf
 ```

6. Insert following configuration into http section

```
upstream myproject {
    server 172.31.56.69;
    server 172.31.58.43;
    server 172.31.61.93;
  }

server {
    listen 80;
    server_name rietta.online www.rietta.online;

    location / {
 proxy_redirect      off;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
      proxy_pass http://myproject;
     }
}
```

![Screenshot 2023-02-02 203538](https://user-images.githubusercontent.com/101978292/216476469-48b32712-1173-457e-86ec-582579b78b40.jpg)



7. We need to test our Nginx config file to know if there is any syntax error.**



```
 sudo nginx -t

```
8. Remove the default page

```
 sudo rm -rf /etc/nginx/sites-enabled/default
 ```

9. We need to test our Nginx config file to know if there is any syntax error.

```
 sudo nginx -t

```

![Screenshot 2023-02-02 203753](https://user-images.githubusercontent.com/101978292/216479987-94ffc0ab-6512-433f-8679-45e406bb7ce4.jpg)


10. We need to link our config file from site-enabled to site-available.

```
 cd /etc/nginx/sites-enabled/

 sudo ln -s ../sites-available/load_balancer.conf .
 
 ls -l

 sudo systemctl reload nginx
```


![23](https://user-images.githubusercontent.com/101978292/216480140-00ce2333-5600-4df5-a7d2-884fcad4a577.jpg)


11. We need to confirm if out load balancer is actually working.

```
http://Load-Balancer-Public-IP-Address/index.php

```

![image](https://user-images.githubusercontent.com/85270361/168774861-0277feae-dc27-47b6-9e6c-4cc43cf70cc8.png)




#### Step 2 - REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

1. Register a domain name with any registrar of your choice in any domain zone on Route53 or any domain name provider (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
2. If registered on another name domain provider, forword the nameserver to Route53 by copying the the nameservers provided by Route53 DNS to the domain name bought on the provider site.
2. Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
3. Create a static IP address, allocate the Elastic IP and associate it with an Nginx_LB EC2 server to ensure your IP remain the same everytime you restart the instance.
4. Update **A record** in your registrar with A record name `www.rietta.online` and `rietta.online` to point to Nginx LB using Elastic IP address


![Screenshot 2023-02-03 005753](https://user-images.githubusercontent.com/101978292/216478012-c65ca183-9adc-4a41-b82f-1eba3e8e9462.jpg)

5. Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://rietta.online


![Screenshot 2023-02-02 192720](https://user-images.githubusercontent.com/101978292/216478117-b1ba11ba-20c8-4c52-a439-9bd58da7930f.jpg)


6. Configure Nginx to recognize your new domain name, update your nginx.conf with server_name www.buildwithme.link instead of server_name www.rietta.online
7. Next, install certbot and request for an SSL/TLS certificate, first install certbot dependency: 

```
sudo apt install python3-certbot-nginx -y
```
8. Install certbot: 
```
sudo apt install certbot -y

```
9. Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it on step 4).

```
sudo certbot --nginx -d rietta.online -d www.rietta.online
```

![Screenshot 2023-02-02 205201](https://user-images.githubusercontent.com/101978292/216479067-31b6615c-d3f4-4b3f-81f7-853b79bd9510.jpg)

10. Test secured access to your Web Solution by trying to reach https://rietta.online , if successful, you will be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string. Click on the padlock icon and you can see the details of the certificate issued for your website.


![Screenshot 2023-02-02 192752](https://user-images.githubusercontent.com/101978292/216479632-699ee07e-0a5e-462a-834f-450f18291652.jpg)

![S](https://user-images.githubusercontent.com/101978292/216479707-6e3bcabe-91c8-4510-acd9-e0017fa9a145.jpg)

![Screenshot 2023-02-02 195335](https://user-images.githubusercontent.com/101978292/216479830-3a966d9b-ee6a-4539-969a-7f943a30fe31.jpg)


# Configure Secure Connection To The Load Balancer

#### Step 3 - Set up periodical renewal of your SSL/TLS certificate

1. By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently. You can test renewal command in dry-run mode: 

```
sudo certbot renew --dry-run

```
2. Best pracice is to have a scheduled job to run renew command periodically. Let us configure a cronjob to run the command twice a day. To do so, edit the crontab file with the following command: 

```
crontab -e

```
3. Add following line: 

```
* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1

```
4. You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression.
