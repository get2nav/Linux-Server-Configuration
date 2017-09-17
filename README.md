# Linux-Server-Configuration

IP: 54.91.64.48
SSH Port: 2200
Complete URL: http://54.91.64.48/

Initial steps to setup lightsail

1. Create the Account in https://lightsail.aws.amazon.com
2. Create an instance by  choosing "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.
3. choose the plan and give the name for the instance
4. Start to the instance
5. Download the ssh-key file for the server
6. Copy the key in the localserver into .ssh folder
7. change key file permissiom

  chmod 400 LightsailDefaultPrivateKey-us-east-1.pem

8. Login using command

  ssh ubuntu@54.91.64.48 -p 22 -i LightsailDefaultPrivateKey-us-east-1.pem


Updating the packages

1. Update the packages use
  $ sudo apt-get update
  $ sudo apt-get upgrade

2.  Finger install
  sudo apt-get install


Changing the Port from 22 to 2200

1. Type in terminal  $ sudo nano /etc/ssh/sshd_config
2. change Port from 22 to 2200
3. change PermitRootLogin  as no
4. then in terminal type  $ sudo service ssh restart


Configuring Firewall

Type the following

$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing
$ sudo ufw allow 2200/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 123/udp
$ sudo ufw enable


In lightsail account in networking table del 80/SSH Port and add 2200/tcp and 123/tcp

account will then get disconnected  

then login using

ssh ubuntu@54.91.64.48 -p 2200 -i LightsailDefaultPrivateKey-us-east-1.pem



Create grader and make sudoer

$ sudo adduser grader
$ sudo nano /etc/sudoers.d/grader

paste this : grader ALL=(ALL) NOPASSWD:ALL

Password less Access:

In local type: ssh-keygen
 specify the folder to save the key


 now in ubuntu
 type the following

$ su --login grader
$ sudo mkdir .ssh
$ sudo nano .ssh/authorized_keys (paste the key here)
$ sudo chmod 700 .ssh
$ sudo chmod 600 .ssh/authorized_keys


 For login as Grader
ssh grader@54.91.64.48 -p 2200 -i ~/.ssh/udacity


Deployment of project:

Changing the UTC time

$ sudo timedatectl set-timezone UTC  

Dependency package installation
$ sudo apt-get -y install python-pip
$ sudo pip install SQLAlchemy
$ sudo pip install psycopg2
$ sudo pip install flask
$ sudo pip install oauth2client
$ sudo pip install requests


 Installation and Configure Apache to serve a Python mod_wsgi, postgres

$ sudo apt-get install apache2
$ sudo apt-get install libapache2-mod-wsgi
$ sudo apt-get install postgresql postgresql-contrib


Database Setup:
1. setup
$ sudo -u postgres psql postgres

2. New User and DB
CREATE USER catalog WITH PASSWORD 'catalog';
ALTER ROLE catalog WITH LOGIN;
ALTER USER catalog CREATEDB;
CREATE DATABASE catalog WITH OWNER catalog;
\c catalog
GRANT ALL ON SCHEMA public TO catalog;
\q
exit
sudo service postgresql restart


Installation of Git, Flask , SQLAlchemy packages

$ sudo apt-get install git
$ sudo apt-get install python-psycopg2 python-flask
$ sudo apt-get install python-sqlalchemy python-pip
$ sudo pip install psycopg2
$ sudo pip install oauth2client
$ sudo pip install requests
$ sudo pip install httplib2
$ sudo pip install flask-seasurf


Setting the catalog Application

1. Creating Flask Folder
  $ cd /var/www
  $ sudo mkdir Flask
  $ cd Flask

2. Getting catalog from git
  $ git clone https://github.com/get2nav/Sport-Catalog.git

3. Go to Sport-Catalog->vagrant->catalog folder and copy only the catalog folder to /var/www

4. Rename catalog folder to Flask

5. Go to Flask and edit file Application.py , database_setup.py and sports_menu.py

change engine = create_engine('sqlite:///sports_db.db') to create_engine('postgresql://catalog:catalog@localhost/catalog')

6. Then Run

$ sudo python database_setup.py
$ sudo python sports_menu.py
$ sudo mv application.py __init__.py
I

Deploy using these deatils


sudo apt-get install libapache2-mod-wsgi python-dev
Next, you will need to enable mod_wsgi Apache module. To do so, run the following command:

sudo a2enmod wsgi

Install Flask
Now, you will need to set up virtual environment that will keep the application and its dependencies isolated from the main system.

You can use pip to install virtualenv and Flask. If pip is not installed, you can install it by running the following command:

sudo apt-get install python-pip
Now, install virtualenv by running the following pip command:

sudo pip install virtualenv
Next run the following command with the name of your temporary virtual environment.

sudo virtualenv Virtenv
Now, install Flask in that environment by activating the virtual environment with the following command:

source Virtenv/bin/activate
Now, run following command to install Flask inside it:

sudo pip install Flask
Next, run the following command to test if the installation is successful and the app is running:

sudo python /var/www/Flask/Flask/__init__.py
You should see the following output:

* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
If you see the above output, you have successfully configured the app.

You can also deactivate the virtual environment by running the following command:

deactivate
Configure New Virtualhost for Flask
Now, you will need to create a new virtual host file for Flask App.

To do so, run the following command:

sudo nano /etc/apache2/sites-available/FlaskApp.conf
Add the following content. Make sure change the ServerName to your domain name:

<VirtualHost*:80>
        ServerName 54.91.64.48
        WSGIScriptAlias / /var/www/Flask/flaskapp.wsgi
        <Directory /var/www/Flask/Flask/>
            Order allow,deny
            Allow from all
        </Directory>
        Alias /static /var/www/Flask/Flask/static
        <Directory /var/www/Flask/Flask/static/>
            Order allow,deny
            Allow from all
        </Directory>
        ErrorLog /var/www/Flask/error.log
        LogLevel warn
        CustomLog /var/www/Flask/access.log combined
</VirtualHost>
Save and close the file.

Enable the virtual host with the following command:

sudo a2ensite FlaskApp
Next, create a flaskapp.wsgi file inside the Flask directory to serve the Flask App.

sudo nano /var/www/Flask/flaskapp.wsgi
Add the following content:

#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/Flask/")

from Flask import app as application
application.secret_key = 'super_secret_key'

Save the file and restart Apache to apply the changes
$ sudo apachectl restart

To test your Flask App, open your favorite web browser and type the URL http://54.91.64.48/

Alternatively, you could use the following curl command in your terminal:

curl 54.91.64.48



Third-party resources:

1. https://devops.profitbricks.com/tutorials/deploy-a-flask-application-on-ubuntu-1404/

2. Udacity Nanodegree Course videos and tutorials

3. https://discussions.udacity.com/t/how-to-login-to-my-aws-virtual-server-as-new-user-grader/201164
