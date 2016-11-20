# Linux Server Configuration

Public IP Address: http://35.163.233.99/
```
ssh -i ~/.ssh/udacity_key.rsa root@35.163.233.99
```

## Step 1
 - Add an new user which its username is "grader" and its password is "grader"
```
sudo adduser grader
```
 - Change the new user permission
 - Run visudo
```
sudo visudo
```
 - Add the following line to the permisssions
```
grader  ALL=(ALL:ALL) ALL
```

 - Allow "grader" to login via private key
```
su grader
sudo mkdir ~/.ssh
sudo chmod 700 ~/.ssh
sudo cp /root/.ssh/authorized_keys ~/.ssh/
sudo chown grader:grader ~/.ssh/authorized_keys
```

 - SSH port and Key Based Authentication
```
sudo nano /etc/ssh/sshd_config
```
 - Firstly, change the SSH port from 22 to 2200 
 - Secondly, change yes to no :
```
PasswordAuthentication yes -> no
```
 - And finally restart ssh service
```
sudo service ssh restart
```

 - Now you are able to login via grader
```
ssh -i ~/.ssh/udacity_key.rsa grader@35.163.233.99
```

## Step 2
- Update all of the packages
```
   sudo apt-get update
```
 - Install GIT
```
sudo apt-get install git
```
 - Install PIP
```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
sudo rm get-pip.py
```
 - Now you can install your python libraries
```
pip install SQLAlchemy Flask flask-seasurf httplib2 oauth2client
```
 - Change directory to www folder and clone Catalog project
```
cd /var/www/
sudo git clone https://github.com/farbod-salimi/catalog.git
```
 - Install Apache and WSGI
 ```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi
```
## Step 3
 - Install PostgreSQL
```
  sudo apt-get install postgresql
  sudo apt-get install python-psycopg2
  sudo -u postgres createuser catalog
  sudo -u postgres createdb catalog
```
## Step 4
 - Setup the apache wsgi module.
```
cp -r /var/www/catalog/catalog.conf /etc/apache2/sites-available/catalog.conf
sudo a2dissite 000-default
sudo a2ensite catalog
sudo service apache2 restart
```

## Step 5
 - Change Firewall configuration
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 2200/tpc
sudo ufw allow www
sudo ufw allow HTTP
sudo ufw allow 80
sudo ufw allow NTP
sudo ufw allow 123
sudo ufw enable
sudo ufw satus
```