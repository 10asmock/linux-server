# Project 6: Linux Server Configuration for Udacity's Full Stack Developer Nanodegree
## ABOUT

A Linux server built on Ubuntu and hosted by Amazon AWS. The server contains an item catalog with users with specific access rights and priviledges.

If you wish to access this site, please visit http://.

## HOW TO USE

### Logging in

Generally, most users need to access the server using ```ssh -i "linux-project.pem" ubuntu@ec2-52-14-242-245.us-east-2.compute.amazonaws.com```. However, for this project, I used the SSH client PuTTY and accessed my server by adding ```ubuntu@ec2-52-14-242-245.us-east-2.compute.amazonaws.com``` on ```Host Name (or IP address)``` found on the ```Session``` tab. Afterward, expand ```SSH``` --> ```Auth``` --> Click ```Browse``` and then I used a private ssh key to login as ```Ubuntu```.

### Add users

- Next, you need to add users by doing ```sudo adduser grader```
- Create a new file in sudoers by doing ```sudo nano /etc/sudoers.d/grader```
- Add the following command ```grader ALL=(ALL:ALL) NOPASSWD:ALL```

### Update all packages

Run two commands:
- ```sudo apt-get update```
- ```sudo apt-get upgrade```

### Change the SSH port from 22 to 2200

- Run ```sudo nano /etc/ssh/sshd_config``` and change port ```22``` to port ```2200```.

### Configure Uncomplicated Firewall (UFW) to allow specific incoming connections for SSH

Run the following:
- ```sudo ufw allow 2200/tcp```
- ```sudo ufw allow 80/tcp```
- ```sudo ufw allow 123/udp```
- ```sudo ufw allow www```
- ```sudo ufw show added```
- ```sudo ufw enable```

### Change timezone to UTC

- Run ```sudo timedatectl set-timezone UTC```

### Configure SSH key based authorization for grader

- 





