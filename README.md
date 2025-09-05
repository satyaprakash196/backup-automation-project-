# backup-automation-project- Web Server Backup Automation

## Overview
This project demonstrates a fully automated backup solution for a production-like environment.
It uses a Bash script to create compressed backups of website files and securely transfer them
to a remote backup server using password-less SSH.

## Features
- Automates daily backups of `/var/www/html/beta`.
- Uses `zip` to compress files for storage optimization.
- Transfers backups to remote server via `scp` with SSH key authentication.
- Configurable scheduling with `cron`.

## Architecture
- **App Server (stapp03)**: Hosts the website files.
- **Backup Server (stbkp01)**: Receives the backup files.

#  App Server (/var/www/html/beta) --> [scp + SSH Key] --> Backup Server (/backup/)

Steps to Setup app-server-3

Install required packages:
sudo yum install zip -y
Configure password-less SSH between App Server and Backup Server:

ssh-keygen -t rsa
ssh-copy-id ec2-user@backup-server

ssh-copy-id is missing, install it:

sudo yum install -y openssh-clients

Verify password-less login
ssh ec2-user@<stbkp01-private-ip>

## Create Required Directories ( App Server (stapp03):
sudo mkdir -p /var/www/html/beta
sudo mkdir -p /backup
sudo mkdir -p /scripts
sudo chown ec2-user:ec2-user /var/www/html/beta /backup /scripts

On Backup Server (stbkp01):
sudo mkdir -p /backup
sudo chown ec2-user:ec2-user /backup


create backup script on app-3 

Create the Backup Script
nano /scripts/beta_backup.sh

Make Script Executable
chmod +x /scripts/beta_backup.sh

Test the Script
Create some dummy data first:
echo "Test file 1" > /var/www/html/beta/index.html
Run the script:
/scripts/beta_backup.sh
Check on App Server:
ls /backup
Check on Backup Server
ls /backup
Automate with Cron
Edit crontab on stapp03:
crontab -e
Add a line to run every day at midnight:
0 0 * * * /scripts/beta_backup.sh


