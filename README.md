# Linux_Sever_Udacity_project
The project is to deploy the Item catalog program on the server by Amazon lightsail

# Details
IP address: 3.8.175.47

User grader key : ssh -i ~/.ssh/key.rsa grader@3.8.175.47 -p 2200

Ubuntu key : ssh -i ~/.ssh/key.pem ubuntu@3.8.175.47 -p 2200

Host name: http://ec2-3-8-175-47.eu-west-2.compute.amazonaws.com/

## URL:
create account in Amazon lightsail [click here](https://aws.amazon.com/).

## summary of software installed
2) Virtual box (last update) [click here](https://www.virtualbox.org/wiki/Downloads)
3) Vagrant (last update) [click here](https://www.vagrantup.com/downloads.html).
4) command line (Git Bash for windows) [click here](https://git-scm.com/downloads).
5) Google & github to search if you stuck in any steps [click here](https://github.com/).

Inside the terminal:
* Python Pip to download the rest framework [click to see the installation code](https://pip.pypa.io/en/stable/installing/).
* pip install flask.
* pip install python .
* pip install postgresql.
* pip install sqlalchemy.
* pip install oauth2client.client
* pip install httplib2
* pip install requests


### summary of configurations made:
1- Create account in Amazon (fill all the required info and put your credit card, The only will take 1 $ to verify the account).
2- open the home page and follow the below:

* create instance, then select the (Instance location) "not mandatory"
* Select a platform (unix/ linux).
* Select a blueprint (OS only) then select (Ubuntu).
* select the lowest payment.
* Identify your instance (Add name for your instance).
* click create.
3- After configuring the instance, Go to home page till the server status change from pending to running.
4- click on the instance and go down to the end of page, you can download your key from there.
click on the *Account page* and other page will open for you to select the region again ,click download.
5- back to the home page -> our instance-> go to the networking bar and add 2 more keys.
* custom key -> TCP -> 123
* custom key -> TCP -> 2200
Then save.
6- make a directory for the project Ex. (linuxproject).
7- Set up the vagrant in your command line (git Bash) by following the below:
```
$vagrant init ubuntu/trusty64
```
```
$vagrant up
```
```
$vagrant provision #if needs
```
```
$vagrant ssh
```

your username will be shown as vagrant@ubuntu. That's fine.

8- Go to download and check the file we downloaded in step 4. move the file to your working file linuxproject.
*Better way to create a separated folder (Ex: key) for this key and copy it inside our terminal and change the key file name as its too long

& change the type also from pem to rsa*

9- in the vagrant terminal write

```
$cd / vagrant
```
```
$cd key
```
```
$cp *.* /home/vagrant/.ssh #.ssh (is a file inside our vagrant Terminal)
```

```
$cd /home/vagrant/.ssh #to check if the file copied there
```

if it's there go back by $cd

10 - write the below command to secure the key.

```
chmod 600 ~/.ssh/key.rsa
```

11- Go inside the server terminal by using the ssh by the rsa key and IP address.

```
ssh -i ~/.ssh/key.rsa ubuntu@3.8.154.129

```
12- inside the ubuntu user go to the root user to make a change by $sudo su -

*you will need to use sudo in many step to force the terminal to install the framework or create a file*

13- While you are in a root user, create the new user.

```
$sudo adduser grader

```
The terminal will ask you to write the password twice. Then to type several info about the project (not mandatory).

14- to make the user as super user we have to add him to the sudoers.d file by typing the below
```
$sudo nano /etc/sudoers.d/grader
```
The terminal will open and type:
```
grader ALL=(ALL) ALL
```
Exit then save with the same file name.

*Advice*
There are some references saying to type (grader ALL=(ALL:ALL)) for UTF-8 won't fit and it will give error all the time.

15- make the server update by typing:

```
$sudo apt-get update
$sudo apt-get upgrade
$sudo apt-get dist-upgrade
sudo apt-get install finger

```

once the update running the terminal will ask if you need to update a new version of one file. Just select (keep the local one).

16- Go back to vagrant@ubuntu terminal and create the ssh-keygen for the user by typing:

```
$ssh-keygen -f ~/.ssh/id_rsa #select the file name you want
```

17 - write the below to access to the file and copy the key.
```
cat ~/.ssh/id_rsa.pub
```

17 - write the below to access to the file and copy the key.
```
cat ~/.ssh/id_rsa.pub
```

18- Go to the server terminal

```
$ssh -i ~/.ssh/key.rsa ubuntu@3.8.154.129
$sudo su -
```
open the cd/home/grader , follow and type the below in order:

```
$mkdir .ssh
$touch .ssh/authorized_keys
$nano .ssh/authorized_keys

```
```
$ sudo chmod 700 /home/grader/.ssh #change the permissions
$ sudo chmod 644 /home/grader/.ssh/authorized_keys  #change the permissions
```

```
$ sudo chown -R grader:grader /home/grader/.ssh
```



```
sudo service ssh restart

```


```
$ssh -i ~/.ssh/key.rsa grader@3.8.154.129
$sudo nano /etc/ssh/sshd_config #change the port from 22 to 2200

```

```

$sudo service ssh restart
$ssh -i ~/.ssh/key.rsa grader@3.8.154.129 -p 2200 #after changing the port

#to secure the firewall

$ sudo ufw allow 2200/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 123/udp
$ sudo ufw enable
sudo ufw status #to check the status
```


**Application Deployment**
1- start by installing the below:
```
$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi python-dev
$ sudo apt-get install git
$sudo a2enmod wsgi              #enable the wsgi
$ sudo service apache2 restart  #this have to be use always after making any change
```
2- We need to deploy the system to create a separated folder for that and name it.
```
$ cd /var/www
$ sudo mkdir catalog
$ sudo chown -R grader:grader catalog   #to give the access only to grader
$ cd catalog
$sudo service apache2 restart
$git clone [repository url] catalog   #clone the repository inside the catalog file.
$ sudo nano catalog.wsgi   #to create the wsgi for the catalog and fill it by the below.

```
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
application.secret_key = 'super_secret_key'
```
3- Rename the catalog main application to ```__init__.py```.
4- install the virtual in /var/www/catalog
```
$ sudo pip install virtualenv
$ sudo virtualenv venv
$ source venv/bin/activate
$ sudo chmod -R 777 venv
```
5- install the required applications and the framework you need as mention in the installation step.

6- open the ```__init__.py```, Item_catalog_db.py and Item_catalog_seeder.py and change the engine to /var/www/catalog/catalog/client_secrets.json.
```
*Also in gconnect function Also inside the __init__.py*
```
7- To configure and enable our virtual host to run the site type:

```
sudo nano /etc/apache2/sites-available/catalog.conf # write the belo inside it.

```

```
<VirtualHost *:80>
    ServerName 3.8.175.47
    ServerAlias ec2-3-8-175-47.eu-west-2.compute.amazonaws.com
    ServerAdmin admin@3.8.175.47
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
```
$ sudo a2ensite catalog.conf and #enable the virtual host
$ a2dissite 000-default.conf #disable the default host
```

8- To set up the database we need to install the postgresql database

```
$ sudo apt-get install libpq-dev python-dev
$ sudo apt-get install postgresql postgresql-contrib
$ sudo su - postgres -i
$ psql # after typing this the command will change the code and will start by postgres=#
```

```
=#CREATE USER catalog WITH PASSWORD 'password'; #after the terminal replied by role created you can type the rest.
=#ALTER USER catalog CREATEDB;
=#CREATE DATABASE catalog with OWNER movie;
=# \c catalog
=# REVOKE ALL ON SCHEMA public FROM public;
=# GRANT ALL ON SCHEMA public TO catalog;
=# q
$ exit
```

9- to fill the database we have to run the py files by typing:

```
#python /var/www/catalog/catalog/Item_catalog_db.py
#python /var/www/catalog/catalog/Item_catalog_seeder.py
$sudo service apache2 restart
```

**References:**
* https://github.com/mulligan121/Udacity-Linux-Configuration/blob/master/README.md
* https://github.com/iliketomatoes/linux_server_configuration
* http://www.nmonitoring.com/ip-to-domain-name.html?ip=3.8.154.129&pingsub=1&ln=en
* https://github.com/SDey96/Udacity-Linux-Server-Configuration-Project#143-setting-up-the-virtualhost-configuration
* https://www.computerhope.com/unix/uchmod.htm
* https://github.com/HOllarves/Udacity-Linux-Configuration
* https://github.com/adityamehra/udacity-linux-server-configuration
* https://github.com/boisalai/udacity-linux-server-configuration/blob/master/README.md
* https://github.com/rrjoson/udacity-linux-server-configuration
* https://askubuntu.com/questions/907014/gives-error-job-for-apache2-service-failed-because-the-control-process-exited/907306
* https://www.computerhope.com/issues/ch000766.htm


## Author
Nora Almotairy
