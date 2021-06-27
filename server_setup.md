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
