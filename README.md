# Linux-Server-Configuration
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
