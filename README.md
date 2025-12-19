## About the project

In this project I decided to setup a simple lab environment where I could test the functionalities of Wazuh.

The project consists of 2 Ubuntu servers, 1 server is acting as the SIEM server where Wazuh is located and all logs and data will be sent to this server for processing and analyzing.

The other server is acting as the client server. It is running Samba with two shares, one public and one private one.\
As well as DVWA or Damn Vulnerable Web Application project so I can easily monitor and exploit web vulnerabilities.

Both servers are accessible via SSH only if the public keys are located in the *authorized_keys* file and password authentication via SSH is disabled completely.\
I also disabled the *root* user and only the admins can use `sudo` to access it.

---

[SIEM Server Configuration](siem-server-configuration.md)\
[Client Server Configuration](client-server-configuration.md)\
[DVWA Configuration](dvwa-configuration.md)
