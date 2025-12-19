## Installation

DVWA requires some dependencies before running so I had to install those:
```Shell
sudo apt install apache2 -y
sudo apt install mysql-server -y
sudo apt install php php-mysql php-gd libapache2-mod-php php-curl -y
```

After downloading I enabled ***apache2*** so it runs on startup.
```
sudo systemctl enable --now apache2
```

As well I ran a helper program to help me securely setup MySQL server on the client machine.
```Shell
sudo mysql_secure_installation
```

Next is to clone DVWA repository into */var/www/html/* so Apache can serve it over HTTP.
I adjusted permissions for *DVWA* so *www-data* user has ownership and permissions over the directory.
```Shell
sudo chown -R www-data:www-data /var/www/html/DVWA
sudo chmod -R 755 /var/www/html/DVWA
```

## Setting up the database

DVWA uses MySQL to store information. I need to create a new database, a new user and give all privileges to that user on the created database.
```SQL
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Configuration files

I needed the PHP configuration for DVWA to run, so I copied the sample configuration and renamed it to *config.inc.php*, and then updated it with correct credentials for the database I just made.
```PHP
$_DVWA[ 'db_user' ] = 'dvwa';  
$_DVWA[ 'db_password' ] = 'password';  
$_DVWA[ 'db_database' ] = 'dvwa';
```

I also had to enable Apache's rewrite module since DVWA uses URL rewriting.
```Shell
sudo a2enmod rewrite
sudo systemctl restart apache2
```

After all this DVWA is reachable under the address: `http://<serverIP>/DVWA`

To enable Apache to send logs to Wazuh I added to */var/ossec/etc/ossec.conf* under `<ossec_config>` tags:
```XML
<localfile>
	<log_format>apache</log_format>
	<location>/var/log/apache2/access.log</location>
</localfile>

<localfile>
	<log_format>apache</log_format>
	<location>/var/log/apache2/error.log</location>
</localfile>
```
