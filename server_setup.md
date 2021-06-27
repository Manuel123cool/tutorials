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
