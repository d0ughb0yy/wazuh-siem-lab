## Overview

This document covers the installation and configuration of DVWA (Damn Vulnerable Web Application) on the client server, including Apache, MySQL, and PHP setup, along with Wazuh agent integration for log monitoring.

## Installation

DVWA requires several dependencies before running. Install the required packages:

```bash
sudo apt-get install apache2 -y
sudo apt-get install mysql-server -y
sudo apt-get install php php-mysql php-gd libapache2-mod-php php-curl -y
```

Enable Apache to start on boot:

```bash
sudo systemctl enable --now apache2
```

Secure the MySQL installation:

```bash
sudo mysql_secure_installation
```

## Database Setup

DVWA uses MySQL to store data. Create a dedicated database, user, and grant appropriate privileges:

```sql
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Configuration

### DVWA Configuration

DVWA requires a PHP configuration file. Copy the sample configuration and update it with the database credentials:

```php
$_DVWA[ 'db_user' ] = 'dvwa';  
$_DVWA[ 'db_password' ] = 'password';  
$_DVWA[ 'db_database' ] = 'dvwa';
```

### Apache Configuration

Enable Apache's rewrite module, as DVWA uses URL rewriting:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

To serve DVWA as the root of the website, edit `/etc/apache2/sites-enabled/000-default.conf`:

```apache
DocumentRoot /var/www/html/DVWA/
```

After these changes, DVWA is accessible at `http://<serverIP>/`

## Wazuh Agent Integration

Configure the Wazuh agent to monitor Apache logs by editing `/var/ossec/etc/ossec.conf`:

```xml
<localfile>
  <log_format>apache</log_format>
  <location>/var/log/apache2/access.log</location>
</localfile>

<localfile>
  <log_format>apache</log_format>
  <location>/var/log/apache2/error.log</location>
</localfile>
```

## Verification

1. Navigate to `http://<serverIP>/` in a web browser
2. Confirm DVWA loads successfully
3. Log in with the default credentials (admin / password)
4. Navigate to the **Setup** page and click **Create / Reset Database**

## Next Steps

Return to [README.md](../README.md) for an overview of the complete lab architecture.
