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
