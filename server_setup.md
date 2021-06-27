### Server setup tutorial

To connect to your server type root@your_ip (Warnig, when first connect, there has to be root)

```
ssh root@10.10.10
```
Then add user.
```
adduser your_name
```
Dont use: useradd.
Then add sudo.
```
usermod -aG sudo your_name
```
Change user.
```
su your_name
```
Now you can connect with user name.
```
ssh your_name@10.10.10
```
Update your server.
```
sudo apt update
```
Install apache.
```
sudo apt install apache2
```
Setup firewall.
```
sudo ufw allow ssh
sudo ufw allow 'Apache Full'
sudo ufw enable
```
Install python (if needed).
```
sudo apt install python3
sudo apt install python3-pip
```
Install php.
```
sudo apt-get install php
```
Install git.
```
sudo apt-get install git-all
```
#### Setup domain
Here link to godaddy and digital ocean tutorial: [Tutorial](https://medium.com/@seanconrad_25426/connecting-a-godaddy-domain-to-a-digitalocean-droplet-cb1ed5662d58)

Warning: When create A type dns record, use @ and www for the two hostnames, dont type in manually.
And, when copying digital ocean dns servername, delete at the and the point(.).

#### Setup ssl
Tutoral: [Tutorial](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04-de)

From now on your_domain should be replaced width your whole damain: dont forget the (.com) .

```
sudo mkdir /var/www/your_domain
sudo chown -R $USER:$USER /var/www/your_domain
sudo chmod -R 755 /var/www/your_domain
sudo vim /var/www/your_domain/index.html
```
Put this into the index.html:
```
<html> <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
```
Then :wq
```
sudo vim /etc/apache2/sites-available/your_domain.conf
```
Put this into your_domain.conf:
```
<VirtualHost *:80>
       
ServerAdmin webmaster@localhost
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<Directory /var/www/your_domain/>
Options Indexes FollowSymLinks AllowOverride All
Require all granted
</Directory>
```
Then :wq

```
sudo vim /etc/apache2/apache2.conf
```
Put this in the first line:
```
ServerName your_domain
```
Find this in the appche2.conf.
```
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
And allow override, change (AllowOverride None) to (AllowOverride All).
