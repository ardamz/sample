# Web Solution With WordPress

 ## A.  **Preparing The Web & DB Servers**

> I utilised AWS EC2 as my server for this project. 

1. I created 3 additional volumes and attched them to the web server.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/Volumes.png)

2. I used the `gdisk` utility to create a single partition on each of the 3 disks for both the Web Server and DB Server.
 ```bash 
sudo gdisk /dev/xvdf
```
3. I ran the `lsblk` command to view the newly configured partition on each of the 3 disks on both servers.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/DrivesPartitioned.png)

4. I installed the `lvm2` utility by running;
```bash
sudo yum install lvm2
```
I then checked for avalable partitons by running;
```bash 
sudo lvmdiskscan
```
5.  I used the `pvcreate` utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/PhysicalVolumes.png)


![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/SudoPvs.png)
> I verified creation PVs by running ` sudo pvs`

6.  I used the `vgcreate` utility to add all 3 PVs to a volume group (VG) called `webdata-vg`

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/VGCreate.png)

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/SudoVGS.png)

7. On the Web Server, I used th `lvcreate` utility to create 2 logical volumes. apps-lv (using half of the PV size), and logs-lv using the remaining space of the PV size. 

```bash
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv 14G webdata-vg
```
>apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/AppsLV.png)
> I verified creation LVs by running ` sudo lvs`

8. On the DB Server, I used th `lvcreate` utility to create 2 logical volumes. db-lv (using half of the PV size), and logs-lv using the remaining space of the PV size. 

```bash
sudo lvcreate -n db-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv 14G webdata-vg
```
>apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/DBlv.png)
> I verified creation LVs by running ` sudo lvs`

9. I verfied my setup so far by running `sudo lsblk`

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/FinalSetup.png)
>looking good...

10. using the `mkfs` utility, I formated the Logical Volumes (LVs) on both servers with the ext4 filsesystem.

```bash
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/FileSystem.png)
> I swapped `apps-lv` for `db-lv` when runing same command on the DB Server

11. On the Web Server, using the `mkdir` command I created the `/var/www/html` directory to store website files, and the `/home/recovery/logs` directory to store backup of log data. 

```bash
sudo mkdir -p /var/www/html
sudo mkdir -p /home/recovery/logs
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/WebDirectory.png)

12. On the DB Server, using the `mkdir` command I created the `/db` directory to store website files, and the `/home/recovery/logs` directory to store backup of log data. 

```bash
sudo mkdir -p /db
sudo mkdir -p /home/recovery/logs
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/DBdirectory.png)

13. On the Web Srver, I mounted the `/var/www/html` directory on the `appls-lv` logical volume by running;

```bash
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/MountHtml.png)

14. On the DB Srver, I mounted the `/db` directory on the `db-lv` logical volume by running;

```bash
sudo mount /dev/webdata-vg/db-lv /db/
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/MountDB.png)

15. On both servers, I used `rsync` utility to backup all the files in the log directory `/var/log` into `/home/recovery/logs` (This is required before mounting the file system).

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/rsync.png)
> sudo rsync -av /var/log/. /home/recovery/logs/

16. I then proceeded to mount `/var/log` on `logs-lv` logical volume. (Note that all the existing data on `/var/log` will be deleted. That is why step 15 above is very
important).

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/Mountlogs.png)
> sudo mount /dev/webdata-vg/logs-lv /var/log

17. I restored log files back into the `/var/log` directory

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/restore.png)
> sudo rsync -av /home/recovery/logs/. /var/log

18. I updated the `etc/fstab` file with the UUID of my LVs
```bash
sudo blkid
sudo vi /etc/fstab
```

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/blkid.png)

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/fstab.png)


19. I tested the configuration and reloaded the daemon by running;

 ```bash
 sudo mount -a
 sudo systemctl daemon-reload
 ```
I then verified my setup by running the `df -h` comand as shown below.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/dfh.png)



## B.  **Installing WordPress on The Web Server**

1. I updated the repository by running;

```bash
sudo yum -y update
```

2. Install wget, Apache and it's dependencies by running;
```bash
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/wget.png)

3. I then started the Web Server

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```

4. I ran the following commands to install PHP and its dependencies.
```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```
5. I then restarted the Web Server

```bash
sudo systemctl restart httpd
```

6. I ran code lines below to install wordpress and copy wordpress to `/var/www/html`

```bash
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php
sudo cp -R wordpress /var/www/html/
```
7. And some more configuration on the server.

```bash
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```


## C.  **Install MySQL On The DB Server**

1. I updated the repository by running;

```bash
sudo yum -y update
```

2. Installed MySQL Server by running;

```bash
sudo install mysql-server
```

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/mysql.png)

3. I checked the status of the service by running
```bash
sudo systemctl status mysqld
```
And to ensure that the service will be running even after a reboot, I ran the following lines;

```bash
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/status.png)



## D.  **Configuring The DB Server To Work With WordPress**

By running the `sudo mysql` command I was able to interact with the MySQL server and able to do the following;

1. Create a Database,
2. Create a user and 
3. Grant the user access to all tables in the Database.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/DBadmin.png)



## E.  **Configure WordPress To Connect To Remote Database.**

1. I installed MySQL client on the the Web Server to enable it connect to and access the database on the DB Server.

```bash
sudo yum install mysql
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/MysqlClient.png)

2. I connected to the DB Server from the Web Server by running;

```bash
sudo mysql -u ardamz -p -h 172.31.86.130
```
I was able to run the `SHOW DATABASES` command in the `mysql` prompt successfully.

3. I was finally able to access from my browser the link to my wordpress. 

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/6.%20Project%206%20Web%20Solution%20with%20WordPress/WebView.png)


## F.  **Challenges.**

This was a very interesting (stressful) one. My major challenge was juggling between the 2 servers, and on my first run i ended up installing the MySQL server on the web server and the client on the DB server.

I also skipped some commands before mounting my LVs like the backing up of the log files.

On the second run extra care was taken, but I just couldnt get WordPress on the browser. from tinkering with my config file too many times to count or even restarting the httpd service and even the EC2 instances. Apparently i was still using the IP address of the first run of the project. Still dont know how that happened.

It was a nice learning experience. learnt to pay extra attention to details.
