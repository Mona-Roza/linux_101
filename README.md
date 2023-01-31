### Getting Started
   In this section we'll talk about the key pairs, commands and packages when we are using server. If you have any ideas or feedbacks about my notes, you can type in issues, you'll help me and the other learners. 


### Create Instance with AWS
If you want to use this service, you should log in your AWS console.

After logging in your console you should follow these tab parts:
> Services->Compute->EC2->Launch instance

In this tab, you can create a new instance/machine. We are using **Ubuntu 18.4**. So what we will talk about in the other sections will be explained through this version of ubuntu. After this explanation we can flow directions:

**!** This directions explain AWS free services. If you need different system requirements, you can change your system elements except the operating system.

1. Firstly choose name for your server:

[![](/images/names_and_tags.png)](/images/names_and_tags.png)

2. Choose your machine's operating system like that:

[![](/images/machine_image.png)](/images/machine_image.png)


3. Choose your instance type like that:

[![](/images/instance_type.png)](/images/instance_type.png)

 
4. We need a key pair because we can't create password for root user when we are creating our server:


[![](/images/key_pair.png)](/images/key_pair.png)


5. When you are creating your key pair, you should be careful about your key's extension. Because you need your_key.ppk file. But if you hadn't created your key with .ppk extension, don't worry about that. Because, I will explain converting your key file from .pem to .ppk in the other section. 

[![](/images/create_key_pair.png)


6. If your Summary part like that, we can launch our instance:

[![](/images/launch_instance.png)](/images/launch_instance.png)


#### Converting key pair using PuTTy:
In this section we'll convert our key with .ppk extension to key with .pem extension. But before go on the section, you need PuTTy application. So you should install PuTTy.


1. Start PuTTygen.


2. Open your file with .pem extension with using Load button. After that, you can click the save public key button.

[![](/images/convert_key.png)


3. Save public key with Save button. But don't forget where you save.

#### Connect with PuTTy:
In this section we'll access our server with our key pair, remove with accessing key pair, allow accessing with password and create a password for root:


1.  Your ip here: 

[![](/images/ip_address.png)](/images/ip_address.png)


2. Follow **Connection/SSH/Auth** path and browse directory where you saved your key. And after that Click open button.

[![](/images/select_key_file.png)](/images/select_key_file.png)


3. You should login as ubuntu where opened command line window:
`login as: ubuntu`


4. Start editing ssh_config file with `sudo nano /etc/ssh/sshd_config  ` command.


5. Find `PasswordAuthentication` permission and change the value to yes. After that append this  permission : `PermitRootLogin yes` 


6. Close and save editing the file with CTRL + X shortcut.


7. Create password for root with this command: `sudo passwd root`


8. We changed permissions so we have to restart sshd service. Use `sudo service sshd restart` or `sudo systemctl restart sshd` command.


9. Update your system with this command: `sudo apt update && sudo apt upgrade -y`
Also run this command whenever you log in your server.


**!** If you want to add user, you can follow next steps.


10. Create a user with `sudo adduser user-name` command.


11. You can change this user's permissons with `sudo usermod -aG sudo user-name` command. This command gives root permissons to your user. 


12. You can change password for your user with `sudo passwd user-name` command.


With this directions we edited our machine's permissions for accessing with password. After that we can access our server with using any SSH client for Windows machine or with using `ssh root@ip-address` command in your machine's command line for MacOS or Linux machine.

### Some Commands on Linux: 

|**Command**   | **Description**  |  
|---|---|
|apt remove package_name      | Deletes package_name package  |  
|apt purge package_name    | Deletes the residual files of the package named package_name  |  
| sudo ufv status  |  Checks if the firewall is active |  
| sudo ufw allow port_number | Firewall allows port named port_number |
| sudo ufw enable/disable | Activate/deactivate the firewall |
| mkdir folder_name | Creates folder_name folder |
| touch file_name | Creates file_name file |
| cp file1.extension file2.extension | Overwrites file1 with file2. file1 is preserved |
| mv file1.extension file2.extension | Overwrites file1 with file2. file1 is not preserved |
| screen | Openes a screen. You can exit screen with CTRL+A+D shortcut (But your screen isn't killed) |
| screen -ls | Lists open screens |
| screen -r screen_number | Opens screen_number screen |
| CTRL + A and then pres K | Kills the open screen | 
| zip -r file_name.zip * | Zip all files in the folder it is in |
| unzip file_name.zip | Exports file_name.zip |
| top | Monitorizes all processes and process numbers, if you want to monitorize your ram usage, you can press `F` in this screen after that select MEM with arrow keys, save with `S` key and turn back with `Q` key. |
| kill -9  process_number| Terminates process with its process_number |
