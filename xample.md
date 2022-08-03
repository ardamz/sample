# LAMP mean Linux Apache Mysql PHP/Python
 ## I am using an ubuntu OS in a virtual box

To update the OS repositories, run

```bash
sudo apt update
```
![Screenshot](https://github.com/ardamz/pikso/blob/138ee8a6bb5d000924c45fad28b4281f1a6d12e2/LAMP/Update%20package%20manager.png)

To Instal the Apache (web) server, run

```bash
sudo apt install apache2
```

![Screenshot](https://github.com/ardamz/pikso/blob/778b8556b85d3afe7b32183b4ef5c1912e12f5c2/LAMP/install%20apache2.png)

To verify if the apache server is installed and staus, run

```bash
sudo systemctl status apach2e
```

To install a mysql-server which will serve as the database of the stack, run

```bash
sudo apt install mysql-server
```
![Screenshot](https://github.com/ardamz/pikso/blob/778b8556b85d3afe7b32183b4ef5c1912e12f5c2/LAMP/install%20mysql.png)

To verify mysql-server is running and to change te password for the root user, run

```bash
sudo mysql 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'NEWPASSWORD';
exit
```

For additional security config, run the command below, and respond to the prompts as neccessary

```bash
sudo mysql_secure_installation
```

To install PHP and all dependencies for both  mysql and Apache, run

```bash
sudo apt install php libapache2-mod-php php-mysql
```
![Screenshot](https://github.com/ardamz/pikso/blob/778b8556b85d3afe7b32183b4ef5c1912e12f5c2/LAMP/install%20PHP%20and%20dependecies.png)

And to verify if PHP has installed properly, run the follwing code to check the verson of PHP insatlled

```bash
php -v
```

![Screenshot](https://github.com/ardamz/pikso/blob/778b8556b85d3afe7b32183b4ef5c1912e12f5c2/LAMP/verified%20PHP%20installation.png)
