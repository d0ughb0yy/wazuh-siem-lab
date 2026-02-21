## About the project

In this project I decided to set up a simple lab environment where I could test the functionalities of Wazuh.

The project consists of 2 Ubuntu servers. One server acts as the SIEM server where Wazuh is located, and all logs and data are sent to this server for processing and analyzing.

The other server acts as the client server. It runs Samba with two shares (one public and one private), as well as DVWA (Damn Vulnerable Web Application) so I can easily monitor and exploit web vulnerabilities.

Both servers are accessible via SSH only if the public keys are located in the *authorized_keys* file, and password authentication via SSH is disabled completely. The *root* user is disabled, and only admins can use `sudo` to access it.

---

- [SIEM Server Configuration](siem-server-configuration.md) - Setting up Wazuh and hardening the SIEM server
- [Client Server Configuration](client-server-configuration.md) - Configuring Suricata and the Wazuh agent
- [DVWA Configuration](dvwa-configuration.md) - Installing and configuring Damn Vulnerable Web Application
