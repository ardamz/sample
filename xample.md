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
![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/verify%20nginx%20install.png)

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
>This should return a "...syntax is ok" message if all went well.
```bash
sudo unlink /etc/nginx/sites-enabled/default
```
```bash
sudo systemctl reload nginx
```

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/codess.png)

I created a simple index.html file to serve as the root of the new website by running:

```bash
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlemp/index.html

```

I then verified if the websites are served by the new web directory by inpuuting the Public IP address in a browser, and the result was the page below, which is a graphical representaion of the simple index.html that was written. 

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/LEMP%20nginx%20webpage.png) 

>This shows that websites are being served by the preojectlemp web root directory.

To test if the nginx can correctly serve .php files, I created a test PHP file in the document root,

```bash
sudo nano /var/www/projectLEMP/info.php
```
and inserting the following text into the file.

```bash
<?php
phpinfo();
```
![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/PHP%20info%20file.png) 

To verify this, I added the  __*/info.php*__ suffix to the Public IP address of the Linux system.

![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/PHP.%20verfied.png) 

>The result is a web page containing detailed information about the server as shown above.

As the page generated contains sensitive information about the PHP server, it was deleted after reviewing the information on it by running the command below.


```bash
sudo rm /var/www/your_domain/info.php
```
![Screenshot](https://github.com/ardamz/PBL/blob/be1ebed8ae4f7a4720334a7f49e1305326b9eef5/PROJECT%202:%20LEMP%20images/more%20codes.png) 
