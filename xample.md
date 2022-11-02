# Shell Scripting Project

 ## 1.  **Linux**

> I utilised AWS EC2 as my server for this project.

I created and navigated to the `shell` directory by running the following command

```bash
mkdir shell && cd shell
```

I the created and populated a `names.csv` file by running

```bash
vim names.csv
```

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/code1.png)

I then proceeded to create the developers group by running the command below

```bash
sudo groupadd developers
```

I then navigated to `.ssh` directory by running 

```bash
cd ~/.ssh
```

I then created and populated the `id_rsa` and `id_rsa.pub` files by running the `touch` and `vim` command.

I also saved a copy of the private key as `.pem` file locally. This would be used to login as any of the created users using SSH.

```bash
touch id_rsa id_rsa.pub
```

```bash
vim id_rsa.pub
```
```bash
vim id_rsa
```

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/code2.png)


 ## 2. **Automation Script**

I navigated back to the `shell` directory and created a shell script called `onboarding.sh`. The script will perform the following tasks;

1. Read the `names.csv` file, create each user on the server, and add to the existing group called `developers`.

1. Check for the existence of the user on the system, before it will attempt to create it.

1. Ensure that the user that is being created also has a default home folder

1. Ensure that each user has a `.ssh` folder within its HOME folder. If it does not exist, it creates one.

1. For each user’s SSH configuration, create an authorized_keys file and  ensure it has the public key of my current user.

```bash
cd ~/shell
```

```bash
vim onboarding.sh
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/Onboarding.sh.png)

I then changed the permission of the `onboarding.sh` file to make it executable by running the `chmod` command, before running the script.

```bash
chmod +x onboarding.sh
```

```bash
./ onboarding.sh
```
![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/code3.png)


I was able to get the script to run successfully aftersome initial jitters.


![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/Greeeeen.png)
> All tasks accomplished by script.


## 3. **Random User Credential Testing**

I was able to verify that the script ran successfully by doing any of the following;

1. Using the `su` command to switch to other users on the server.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/SwitchUser.png)

1. Using the `ls` command to list all the users with an home fo to other users on the server.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/UsersHomeDirectory.png)


Using the `Santos.pem` file created earlier, i was able to login as various users on the server.

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/SSH@Santos.png)
>SSH as Santos

![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/SSH@Yahaya.png)
>SSH  as Yahaya

## 4. **Hiccups**

Everything was going smoothly until i ran my script, and i got a message i wasn't expecting on the screen.



![Screenshot](https://github.com/ardamz/PersonalDemos/blob/main/Aux_Project_1%20(Shell%20Scripting)/hiccups.png)
> Last 2 lines were unexpected

Apparently for the fifth task of my script, the public was in my `~/.ssh` directory, but the script specified the `~/shell` directory. 

To fix this, i could either edit the script to change the location of the public key or copy the public key to the shell directory, I went for the latter.



