# LAMP mean Linux Apache Mysql PHP/Python
 ## I am using an ubuntu OS in a virtual box

To update the OS repositories, run

```bash
sudo apt update
```
![Screenshot] (https://github.com/ardamz/sample/blob/25ff1f80fe2ab8a807ed32d1ecc4616eb313aecc/Update%20package%20manager.png)

To Instal the Apache (web) server, run

```bash
sudo apt install apache2
```

To verify if the apache server is installed and staus, run

```bash
sudo systemctl status apach2e
```

To install a mysql-server which will serve as the database of the stack, run

```bash
sudo apt install mysql-server
```

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

And to verify if PHP has installed properly, run the follwing code to check the verson of PHP insatlled

```bash
php -v
```
