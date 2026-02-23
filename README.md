# Wazuh SIEM Lab

## About the Project

This project demonstrates the setup of a lab environment for testing and evaluating Wazuh SIEM functionality.

The architecture consists of two Ubuntu servers:

- **SIEM Server**: Hosts Wazuh for log processing and analysis
- **Client Server**: Runs DVWA (Damn Vulnerable Web Application) for monitoring and exploiting web vulnerabilities

Both servers are configured with SSH key-based authentication only. Password authentication is disabled, root login is restricted, and sudo access is limited to administrative users.

## Documentation

- [SIEM Server Configuration](siem-server-configuration.md) - Setting up Wazuh and hardening the SIEM server
- [Client Server Configuration](client-server-configuration.md) - Configuring Suricata and the Wazuh agent
- [DVWA Configuration](dvwa-configuration.md) - Installing and configuring Damn Vulnerable Web Application
