# LOAD BALANCER SOLUTION WITH APACHE

After implementing the Devops Tooling Website Solution, you may be wondering how a user would visit each of the webservers utilizing three separate IP addresses or three different DNS names. We can also question what the point is of having three servers performing the same thing.


Although the complexity of serving requests from several servers is concealed from the average user when accessing a website via a URL, it is difficult to serve all of the users of websites with millions of daily visitors (like Google or Facebook) from a single web server (it is also applicable to databases, but for now we will not focus on distributed DBs).
When a website is opened in the Internet, a domain name portion of each URL is translated (resolved) to an IP address of a target server that will handle requests. DNS servers handle domain name translation (resolution).

A load balancer is a device or software that distributes network or application traffic across multiple servers to ensure that no single server bears too much demand. This improves the overall performance and availability of the application or service. Load balancers can be hardware-based or software-based, and they can use various algorithms to determine how to distribute the traffic. They also provide features such as health checks to ensure that only healthy servers receive traffic, and they can also terminate SSL/TLS connections to offload that processing from the servers.

![1](https://user-images.githubusercontent.com/101978292/215217624-56acfea7-30fc-4900-8283-f2946a0884ea.jpg)

### Step 1 Configure Apache As A Load Balancer 
1. Create an Ubuntu Server 22.04 EC2 instance and name it **Project-8-apache-lb**.
2. Open TCP port 80 on **Project-8-apache-lb** by creating an Inbound Rule in Security Group.
3. Connect to the server through the SSh terminal and install Apache Load balancer then configure it to point traffic coming to LB to the Web Servers by running the following:
```
sudo apt update
sudo apt install apache2 -y
sudo apt-get install libxml2-dev
```
4. Enable the following and restart the service:
```
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic

sudo systemctl restart apache2
```
5. Ensure Apache2 is up and running: `sudo systemctl status apache2`

![1](https://user-images.githubusercontent.com/101978292/215211540-6ff7fabc-810a-481d-b8e1-385c2c745a9c.jpg)


7. Next, configure load balancing in the default config file: 

```
sudo vi /etc/apache2/sites-available/000-default.conf
```


8. In the config file add and save the following configuration into this section **<VirtualHost *:80>  </VirtualHost>** making sure to enter the IP of the webservers 
```
<Proxy "balancer://mycluster">
               BalancerMember http://172.31.95.40:80 loadfactor=5 timeout=1
               BalancerMember http://172.31.89.249:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
```

![Screenshot 2023-01-27 131250](https://user-images.githubusercontent.com/101978292/215213124-92282a01-93df-4c23-8104-24900894434c.jpg)

8. Restart Apache server: 

```
sudo systemctl restart apache2

```
9. Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser:
`http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php`

![3](https://user-images.githubusercontent.com/101978292/215213498-64d8814d-b155-445e-a2fd-559b70d20689.jpg)

![4](https://user-images.githubusercontent.com/101978292/215216300-13434f94-9a64-4f89-b553-20e894f6e423.jpg)

* The load balancer accepts the traffic and distributes it between the servers according to the method that was specified.
10. In Project-7 I had mounted **/var/log/httpd/** from the Web Servers to the NFS server, here I shall unmount them and give each Web Server has its own log directory: 

```
sudo umount -f /var/log/httpd
```

11. Open two ssh/Putty consoles for both Web Servers and run following command: 
```
sudo tail -f /var/log/httpd/access_log
```
12.  Refresh the browser page `http://http://54.237.49.45/index.php` with the load balancer public IP several times and make sure that both servers receive HTTP GET requests from your LB – new records will appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers – it means that traffic will be disctributed evenly between them.

![5](https://user-images.githubusercontent.com/101978292/215213970-944c2e71-d290-4fc0-a868-cfebed1f9a1f.jpg)


### Step 2 – Configure Local DNS Names Resolution
1. It might be difficult to remember and move between IP addresses, especially if you manage a large number of servers.
This may be resolved by setting local domain name resolution. The easiest way is to use **/etc/hosts file**, although this approach is not very scalable, but it is very easy to configure and shows the concept well. 
2. Open this file on your LB server: 
```
sudo vi /etc/hosts
```
3. Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers
```
172.31.56.69 Web1
172.31.58.43 Web2
172.31.61.93 Web3
```

![2](https://user-images.githubusercontent.com/101978292/215214676-eb27781e-5473-4c69-97ac-b0d987926941.jpg)


4. Now you can update your LB config file with those names instead of IP addresses.
```
BalancerMember http://Web1:80 loadfactor=5 timeout=1
BalancerMember http://Web2:80 loadfactor=5 timeout=1
BalancerMember http://Web3:80 loadfactor=5 timeout=1
```
![2](https://user-images.githubusercontent.com/101978292/215216178-ec5e3807-1249-48ce-8bd6-c0be869f9896.jpg)


4. You can try to curl your Web Servers from LB locally `curl http://Web1` or `curl http://Web2` to see the HTML formated version of your website.


![7](https://user-images.githubusercontent.com/101978292/215216243-7227eeb1-0ed5-4097-b9e4-622a40feae39.jpg)

