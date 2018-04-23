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
- Create a SSH directory for grader by doing ```cd grader``` and ```mkdir .ssh``` 

### Creating SSH keys

- Run ```ssh-keygen```. For this step, you can decide to add a password or leave it blank.
- Once you've generated the keys, go to the grader directory(```/home/grader/.ssh```) and run ```sudo nano authorized_keys``` and paste the public key generated from the local machine.
- You can login to grader by doing ```ssh grader@PublicDNS -i ~/.ssh/add_ssh_key_gen_here.rsa```

### Prevent Host Error 

- Run ```sudo nano /etc/hosts```
- Add this line ```127.0.0.1 ip-52-14-242-245```

### Disable root login

- Run ```sudo nano /etc/ssh/sshd_config```
- Change ```PermitRootLogin prohibit-password``` to ```PermitRootLogin no```
- Run ```sudo service ssh restart```
- You are now unable to login via root and must login as ```grader```.

### Update all packages

Run two commands:
- ```sudo apt-get update```
- ```sudo apt-get upgrade```

### Change the SSH port from 22 to 2200

- Run ```sudo nano /etc/ssh/sshd_config``` and change port ```22``` to port ```2200```
- Afterward, do ```sudo service ssh restart```

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

### Install Apache, Git, and other necessary modules

Run the following:
```
sudo apt-get install apache2
sudo apt-get install git
sudo apt-get install libapache2-mod-wsgi python-dev
sudo apt-get install python-psycopg2 python-flask
sudo apt-get install python-sqlalchemy python-pip
sudo pip install oauth2client
sudo pip install requests
sudo pip install httplib2
sudo pip install flask-seasurf
```

### Enable mod_wsgi

- Enable mod_wsgi with ```sudo a2enmod wsgi```
- Start the app server with ```sudo service apache2 start```

### Setup WSGI files

- Do ```cd /var/www/```
- Create a WSGI file named ```itemcatalog.wsgi``` in the path to the application directory
- Add the following:
```
WSGI file

import sys
import logging

logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, '/var/www/itemcatalog/catalog')

from application import app as application
application.secret_key='super_secret_key'
```

### Setup virtual host

To set up the web server do ```sudo nano /etc/apache2/sites-available/itemcatalog.conf``` and paste the following:

```
Virtual Host file
<VirtualHost *:80>
     ServerName  52.14.242.245
     ServerAdmin hegemon1984@gmail.com
     WSGIScriptAlias / /var/www/itemcatalog/itemcatalog.wsgi
     <Directory /var/www/itemcatalog>
          Order allow,deny
          Allow from all
     </Directory>
     #Allow Apache to deploy static content
     <Directory /var/www/itemcatalog/static>
        Order allow,deny
        Allow from all
     </Directory>
      ErrorLog ${APACHE_LOG_DIR}/error.log
      LogLevel warn
      CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

