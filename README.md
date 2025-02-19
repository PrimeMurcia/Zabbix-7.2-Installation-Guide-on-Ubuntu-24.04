# README: Zabbix 7.2 Installation Guide on Ubuntu 24.04

## Overview
This document provides a step-by-step guide to installing and configuring Zabbix 7.2 on Ubuntu 24.04 with MySQL and Nginx.

## Prerequisites
- Ubuntu 24.04 server
- Sudo or root access
- Internet connection

## Step 1: Update and Upgrade System
```bash
sudo -su
apt update && apt upgrade -y
```

## Step 2: Install MySQL Server
```bash
apt install mysql-server mysql-client -y
```

## Step 3: Install Zabbix Repository
```bash
wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.2+ubuntu24.04_all.deb
apt update
```

## Step 4: Install Zabbix Server, Frontend, and Agent
```bash
apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent -y
```

## Step 5: Configure MySQL Database for Zabbix
```bash
mysql
```
Inside MySQL, execute the following commands:
```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
SET GLOBAL log_bin_trust_function_creators = 1;
QUIT;
```

## Step 6: Import Zabbix Database Schema
```bash
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

## Step 7: Disable Log Bin Trust Function Creators
```bash
mysql -e "SET GLOBAL log_bin_trust_function_creators = 0;"
```

## Step 8: Configure Zabbix Server
Edit Zabbix server configuration file:
```bash
nano /etc/zabbix/zabbix_server.conf
```
Find and set:
```ini
DBPassword=password
```
Save and exit.

## Step 9: Configure Nginx for Zabbix
Edit the Nginx configuration file:
```bash
nano /etc/zabbix/nginx.conf
```
Update the following lines:
```nginx
listen 80;
server_name example.com;
```
Save and exit.

## Step 10: Restart and Enable Services
```bash
systemctl restart zabbix-server zabbix-agent nginx php8.3-fpm
systemctl enable zabbix-server zabbix-agent nginx php8.3-fpm
```

## Step 11: Access Zabbix Web Interface
Open a web browser and go to:
```
http://example.com
```
Follow the setup wizard to complete the installation.

## Step 12: Default Login Credentials
- **Username:** Admin
- **Password:** zabbix

Change the default password after login for security purposes.

## Conclusion
You have successfully installed and configured Zabbix 7.2 on Ubuntu 24.04 with MySQL and Nginx.

## License
This guide is provided as-is without any warranty. Use it at your own discretion.

