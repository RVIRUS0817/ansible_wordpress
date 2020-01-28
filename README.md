# ansible_wordpress

![wordpress-logo-680x400](https://user-images.githubusercontent.com/5633085/44000641-d705b6d0-9e5e-11e8-8431-3fd766c459a3.png)

## Environment
- CentOS7  
- wordpress  
- php7.4
- H2O 2.2.4  
- MySQL5.7  

## 1.setting user


It is assumed that you have set ssh's config with the initial user (root)
In ansible you can specify your favorite user and enable it with sudo no pass so please change the following hoge and add the public key.

・hosts  
・wordpress.yml  
・roles/common/tasks/user.yml

## 2.setting H2O

The web server uses H2O.
httpsization uses letsencrypt.

・roles/h2o/files/h2o.conf  
・roles/h2o/files/conf.d/hoge.conf  
・roles/h2o/tasks/main.yml  
・roles/letsencrypt/tasks/main.yml  

Please change hoge to domain name!!

## 3.dry-run

````
$ ansible-playbook -i hosts wordpress.yml -C
````

## 4.run

````
$ ansible-playbook -i hosts wordpress.yml
````

## 5.setting MySQL

Set up MySQL manually. Let's set up a dedicated DB and authority for wordpress.

・MySQL

```
$ grep 'temporary password' /var/log/mysqld.log
$ mysql_secure_installation
change root pass
all Y
```

・create DB and user,pass

```
$ mysql -u root -p
> create database wordpress;
> grant all privileges on wordpress.* to wordpress@localhost identified by 'xxxxxxxxxxx';
```

## 6.setting letsencrypt

Let's get a certificate with letsencrypt when wordpress can be viewed with http.  
Please change hoge to domain name!!

```
# vim /etc/h2o/conf.d/hoge.jp
#"hoge.jp:443":
Comment out all below
```

```
# systemctl restart h2o  
```

```
# /etc/letsencrypt/letsencrypt-auto certonly --standalone -d hoge.jp --agree-tos

```

## 7.setting wordpress  

```
# cd /var/www/hoge.jp
# wget https://wordpress.org/latest.zip
# unzip latest.zip
# cd ..
# chown -R nobody:nobody hoge.jp
```

Enjoy!!!🤣

