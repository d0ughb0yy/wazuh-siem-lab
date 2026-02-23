## Overview

This document covers the configuration of the client server, including SSH hardening, Suricata IDS installation and setup, and Wazuh agent integration.

## Initial Configuration

### SSH Hardening

The `sshd_config` file in `/etc/ssh/` is configured to accept SSH connections only if the public key is present in the `authorized_keys` file of the `web-admin` user.

## Configuring Suricata

### Installation

Install the required dependencies and add the Suricata stable repository:

```bash
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata jq
```

### Initial Configuration

Replace the `eth0` interface entries in `/etc/suricata/suricata.yaml` with the appropriate network interface (e.g., `enp1s0`). The most important change is in the `af-packet` section:

```yaml
af-packet:
  - interface: enp1s0
```

After making these changes, restart Suricata to load the new settings.

### Wazuh Agent Integration

Configure the Wazuh agent to forward Suricata alerts to the SIEM server by editing `/var/ossec/etc/ossec.conf`:

```xml
<ossec_config>
  <localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>
</ossec_config>
```

This configuration specifies the JSON log format and the location of the Suricata eve.json file for SIEM ingestion.

### Rule Setup

Download rules from the [Emerging Threats website](https://rules.emergingthreats.net/open/suricata/) and obtain the `emerging.rules.tar.gz` file containing the required rule sets.

Edit the Suricata rule configuration to include a wildcard pattern, enabling all `.rules` files in the `/var/lib/suricata/rules/` directory to be loaded automatically.

## Next Steps

Continue to [DVWA Configuration](dvwa-configuration.md) to install and configure the Damn Vulnerable Web Application.