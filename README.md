# Linux-Server-Configuration
This is the final project for Udacity Full Stack Web Developer Nanodegree.

You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.
You will learn in this project how to access, secure, and perform the initial configuration of a bare-bones Linux server. You will then learn how to install and configure a web and database server and actually host a web application.

You can visit http://35.157.59.198/ 

## Start a new Ubuntu Linux Server instance on Amazon Lightsail
1. Create an AWS account
2. Click **Create instance** button on the home page
3. Select **Linux/Unix** platform
4. Select **OS Only** and **Ubuntu**
5. Select an instance plan
6. Name your instance
7. Click **Create** button

## SSH into your Server
1. Click on `SSH keys` tab From the `Account page`on Amazon Lightsail and download the Private Key.
2. Create a new file named **lightsail_key.rsa** under ~/.ssh folder on your local machine
3. Copy and paste content from downloaded private key file to **lightsail_key.rsa**
4. In your terminal, type: `chmod 600 ~/.ssh/lightsail_key.rsa`.
5. SSH into the instance: `$ ssh -i ~/.ssh/lightsail_key.rsa ubuntu@35.157.59.198`

* Public IP address: 35.157.59.198

## Secure the server

### Step 3: Update all currently installed packages

```
sudo apt-get update
```

### Step 4: Change the SSH port from 22 to 2200

- Edit the `/etc/ssh/sshd_config` file: `sudo nano /etc/ssh/sshd_config`.
- Change the port number from `22` to `2200`.
- Save and exit using CTRL+X and confirm with Y.
- Restart SSH: `sudo service ssh restart`.

## Step 5: Configure the Uncomplicated Firewall (UFW)

- Check firewall status: `$ sudo ufw status`
- Set default firewall to deny all incomings: `$ sudo ufw default deny incoming`
- Set default firewall to allow all outgoings: `$ sudo ufw default allow outgoing`
- Allow incoming TCP packets on port 2200 to allow SSH: `$ sudo ufw allow 2200/tcp`
- Allow incoming TCP packets on port 80 to allow www: `$ sudo ufw allow www`
- Allow incoming UDP packets on port 123 to allow NTP: `$ sudo ufw allow 123/udp`
- Close port 22: `$ sudo ufw deny 22`
- Enable firewall: `$ sudo ufw enable`
- Check out current firewall status: `$ sudo ufw status`

## Create a new user account **grader** and give **grader** sudo access
- Create a new user account **grader**:`$ sudo adduser grader` 
- Enter a password and fill out information for this new user.

## Set SSH login using keys
1- On the local machine:
  - Run `ssh-keygen`
  - Enter file in the local directory `~/.ssh`
  - Enter in a passphrase twice. Two files will be generated (  `~/.ssh/grader_key` and `~/.ssh/grader_key.pub`)
  - Run `cat ~/.ssh/grader_key.pub` and copy the contents of the file
  - Log in to the grader virtual machine
- On the grader's virtual machine:
  - Create a new directory called `~/.ssh` (`mkdir .ssh`)
  - Run `sudo nano ~/.ssh/authorized_keys` and paste the content into this file, save and exit
  - Give the permissions.
  - Restart SSH: `sudo service ssh restart`
  

### Step 9: Configure the local timezone to UTC

- Run `$ sudo dpkg-reconfigure tzdata`.

## Install and configure Apache
- Install **Apache**: `$ sudo apt-get install apache2`
- Go to http://35.157.59.198, if Apache is working correctly, a **Apache2 Ubuntu Default Page** will show up

## Install and configure Python mod_wsgi
- Install the **mod_wsgi** package: `$ sudo apt-get install libapache2-mod-wsgi-py3`
- Run `$ sudo a2enmod wsgi` to Enable **mod_wsgi**.
- Run `$ sudo service apache2 restart`


## Install PostgreSQL
- Run `$ sudo apt-get install postgresql`
- Do not allow remote connections.

## Install git
install `git`by run `sudo apt-get install git`.

## Clone and setup the Item Catalog project from the GitHub repository
- Create dictionary: `$ mkdir /var/www/catalog`
- CD to this directory: `$ cd /var/www/catalog`
- Clone the catalog app: `$  git clone https://github.com/mahazz/item-catalog.git catalog`
- Change the ownership: `$ sudo chown -R ubuntu:ubuntu catalog/`
