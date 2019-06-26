# Linux-Server-Configuration
This is the final project for Udacity Full Stack Web Developer Nanodegree.

You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.
You will learn in this project how to access, secure, and perform the initial configuration of a bare-bones Linux server. You will then learn how to install and configure a web and database server and actually host a web application.

You can visit http://18.197.195.222/ 

## Start a new Ubuntu Linux Server instance on Amazon Lightsail
- Create an AWS account
- Click **Create instance** button on the home page
- Select **Linux/Unix** platform
- Select **OS Only** and **Ubuntu**
- Select an instance plan
- Name your instance
- Click **Create** button

## SSH into your Server
- Click on `SSH keys` tab From the `Account page`on Amazon Lightsail and download the Private Key.
- Create a new file named **authorized_keys** under ~/.ssh folder on your local machine
- Copy and paste content from downloaded private key file to **authorized_keys**
- In your terminal, type: `chmod 600 ~/.ssh/authorized_keys`.


* Public IP address: 18.197.195.222

## Secure the server

###  Update all currently installed packages

```
sudo apt-get update
```

###  Change the SSH port from 22 to 2200

- Edit the `/etc/ssh/sshd_config` file: `sudo nano /etc/ssh/sshd_config`.
- Change the port number from `22` to `2200`.
- Save and exit using CTRL+X and confirm with Y.
- Restart SSH: `sudo service ssh restart`.

##  Configure the Uncomplicated Firewall (UFW)

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
- Add privileges to `grader` user by run `sudo visudo` and adding grader under root:
  ```
  root    ALL=(ALL:ALL) ALL
  grader  ALL=(ALL:ALL) ALL
  ```
- To verify `grader` has sudo permissions. Run `su - grader`and run `sudo -l`. The output should be display like this:
 ```
  Matching Defaults entries for grader on ip-172-26-8-215.eu-central-1.compute.internal:
      env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
  
  User grader may run the following commands on ip-172-26-8-215.eu-central-1.compute.internal:
      (ALL : ALL) ALL
      (ALL : ALL) ALL
  ```

## Set SSH login using keys
1- On the local machine:
  - Run `ssh-keygen`
  - Enter file in the local directory `~/.ssh`
  - Enter in a passphrase twice. Two files will be generated (  `~/.ssh/id_rsa.pub`).
  - Run `cat ~/.ssh/id_rsa.pub` and copy the contents of the file. 
  - Log in to the grader virtual machine and the password is (Maha#123).
- On the grader's virtual machine:
  - Create a new directory called `~/.ssh` (`mkdir .ssh`)
  - Run `sudo nano ~/.ssh/authorized_keys` and paste the content into this file, save and exit
  - Give the permissions.
  - Restart SSH: `sudo service ssh restart`
  

###  Configure the local timezone to UTC

- Run `$ sudo dpkg-reconfigure tzdata`.

## Install and configure Apache
- Install **Apache**: `$ sudo apt-get install apache2`
- Go to http://18.197.195.222, if Apache is working correctly, a **Apache2 Ubuntu Default Page** will show Apache welcome page.

## Install and configure Python mod_wsgi
- Install the **mod_wsgi** package: `$ sudo apt-get install libapache2-mod-wsgi-py3`
- Run `$ sudo a2enmod wsgi` to Enable **mod_wsgi**.
- Run `$ sudo service apache2 restart`


## Install PostgreSQL
- Run `$ sudo apt-get install postgresql`
- Make sure does not allow remote connections.
- Switch to the `postgres` user by run `sudo su - postgres`.
- Open PostgreSQL with `psql`.
- Create the `catalog` user with login role and allow user to create database table: :
  ```
  postgres=# CREATE ROLE catalog WITH PASSWORD 'catalog';
  postgres=# ALTER ROLE catalog CREATEDB;
  ```
- List the existing roles: `\du`:
  ```
                                     List of roles
   Role name |                         Attributes                         | Member of 
  -----------+------------------------------------------------------------+-----------
   catalog   | Create DB                                                  | {}
   postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
  ```
## Install git
install `git`by run `sudo apt-get install git`.

## Clone and setup the Item Catalog project from the GitHub repository
- Create dictionary: `$ mkdir /var/www/html/item-catalog `
- CD to this directory: `$ cd /var/www/html/item-catalog `
- Clone the catalog app: `$  git clone https://github.com/mahazz/item-catalog.git`
- Change the ownership: `$ sudo chown -R ubuntu:ubuntu catalog/`
