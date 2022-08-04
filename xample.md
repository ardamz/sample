# LEMP means NGINX Mysql PHP

 ## 1.  **Linux**

> I utilised AWS EC2 as my Linux portion of the stack. I used an Ubuntu server, any Linux distro would equally work.

To update the OS repositories, I ran the following codes

```bash
sudo apt update
```
![Screenshot](https://github.com/ardamz/PBL/blob/8e3fae5a1c536d9bba46c3ca21d7e792d4cf76d4/PROJECT%202:%20LEMP%20images/Update.png)

## 2. **Nginx**

To Install the nginx (web) server, I ran the following codes

```bash
sudo apt install nginx
```

![Screenshot](https://github.com/ardamz/PBL/blob/8e3fae5a1c536d9bba46c3ca21d7e792d4cf76d4/PROJECT%202:%20LEMP%20images/Install%20nginx.png)

And I ran the following code to verify the Apache installation and status

```bash
sudo systemctl status nginx
```
![Screenshot](https://github.com/ardamz/PBL/blob/4d89f68e3290df1e7f542297ee66e59ad113b9d7/PROJECT%202:%20LEMP%20images/verify%20nginx%20install.png)

![Screenshot](https://github.com/ardamz/PBL/blob/4d89f68e3290df1e7f542297ee66e59ad113b9d7/PROJECT%202:%20LEMP%20images/verify%20nginx%20install.png)

To verify if the nginx server is up and running, i just grabbed the Public IP address of the Linux system from the AWS EC2 consoloe and put it in the browser and the (default) page below is displayed

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/verify%20nginx%20working.png)

## 3. **Mysql**

To install a mysql-server which will serve as the database of the stack, I ran the following codes

```bash
sudo apt install mysql-server
```
![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/install%20mysql-server%20.png)

To verify mysql-server is running and to change the password for the root user:

```bash
sudo mysql 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NEWPASSWORD';
exit
```
![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/mysql%20config.png)

For additional security config, i ran the command below, and responded to the prompts as neccessary

```bash
sudo mysql_secure_installation
```

## 4. **PHP**
To install PHP and all dependencies for both  mysql and Apache, I ran the following codes

```bash
sudo apt install php-fpm php-mysql
```
![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/PHP%20Installation.png)

And when prompted, I hit the Y button and pressed ENTER to confirm installation.

>I ran a sequence of codes to do the following:

1. create the root web directory for projectlemp.
2. Change the ownership of the created directory.
3. Open a new configuration file in Nginx’s sites-available directory using the nano command-line editor.

```bash
sudo mkdir /var/www/projectlemp 
```
```bash
 sudo chown -R $USER:$USER /var/www/projectlamp
```
```bash
sudo nano /etc/apache2/sites-available/projectlamp.conf
```

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/codes.png)

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/LEMP%20config%20file.png)

>I also ran another batch of codes to do the following:
1. Activate my configuration by linking to the config file from Nginx’s sites-enabled directory,
2. Test my configuration for syntax errors,
3. Disable default Nginx host that is currently configured to listen on port 80,
4. Reload Nginx to apply the changes.


I also confirmed the creation of the configuration file by running:

```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
```bash
sudo nginx -t
```
>This should return a "...syntax is ok" message if all went weel.
```bash
sudo unlink /etc/nginx/sites-enabled/default
```
```bash
sudo systemctl reload nginx
```

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/codess.png)

then i inserted the following text into the configuration file using the NANO text editor

```bash
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
![Screenshot](https://github.com/ardamz/pikso/blob/778b8556b85d3afe7b32183b4ef5c1912e12f5c2/LAMP/using%20nano%20to%20create%20the%20config%20file.png)

I then ran a series of commands to inform apache to:
1. serve projectlamp using /var/www/projectlampl as its web root directory.
1. disable the default website that comes installed with Apache.
1. To make sure your configuration file doesn’t contain syntax errors, and
1. Finally, reload Apache so these changes take effect.

```bash
sudo a2ensite projectlamp
```

```bash
sudo a2dissite 000-default
```

```bash
sudo apache2ctl configtest
```

```bash
sudo systemctl reload apache2
```

![Screenshot](https://github.com/ardamz/pikso/blob/15064f22af26bf2bc552a48788c2dcbc13be787d/LAMP/server%20cofigured.png)

I created a simple index.html file to serve as the root of the new website by running:

```bash
sudo echo 'Hello LAMP from hostname' 
$(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 
'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```

I then verified if the websites are served by the new web directory by inpuuting the Public IP address in a browser, and the result was the page below, which is a graphical representaion of the simple index.html that was written. 

![Screenshot](https://github.com/ardamz/pikso/blob/993709479fad15bdd620ea7cab8d4b68b2348696/LAMP/projectlamp%20webpage.png)

tToo test the PHP on the website, i changed the order of the PHP configuration file by editing the file using the Vim text editor:

```bash
sudo vim /etc/apache2/mods-enabled/dir.conf
```

![Screenshot](https://github.com/ardamz/pikso/blob/993709479fad15bdd620ea7cab8d4b68b2348696/LAMP/apache2%20defaults%20altered.png)

Then i ran the following codes to 
1. reload the Apache server and 
```bash
sudo systemctl reload apache2
```
2. create a new file named index.php inside your custom web root folder.
```bash
vim /var/www/projectlamp/index.php
```
3. populate the file above with a simple and valid php code
```bash
<?php
phpinfo();
```
I then refreshed the website in my browser, and i got page below which provides information about the  server from the perspective of PHP

![Screenshot](https://github.com/ardamz/pikso/blob/95107fada8a585ba1c57b753179c59f1b3e08009/LAMP/PHP%20verified.png)

 I ran the command below to remove the created php file as it contains sensitive information, and it can easily be recreated in the future if needed.

 ```bash
sudo rm /var/www/projectlamp/index.php
 ```
