## Initial Configuration
### SSH Hardening
I again edited the *sshd_config* file in */etc/ssh/* folder so that the server accepts connections via SSH **only** if the public key is in the *authorized_keys* file of the *web-admin* user this time.

## Configuring Suricata

### Installation
First I needed to download some dependencies and add the repository to my package manager:
```Shell
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata jq
```

### Initial Configuration
I replaced the ***eth0*** entries in */etc/suricata/suricata.yaml* file with my ***enp1s0*** interface, most importantly, change the interface under the ***af-packet:*** section:
```YAML
af-packet:
  - interface: enp1s0
```

After that I restarted Suricata to load new settings and went setting up the Wazuh agent integration.

**Configuring */var/ossec/etc/ossec.conf***
```XML
<ossec_config>
  <localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>
</ossec_config>
```

Add the log format and the location of *eve.json* so alerts are sent to the SIEM server via Wazuh agent.

### Rule setup
I downloaded rules from [Emerging Threats website](https://rules.emergingthreats.net/open/suricata/) and downloaded the *emerging.rules.tar.gz* file containing all the rules I will need.

So I don't need to edit the default rule path I downloaded them to *web-admin* home where I created a new ***suricata-rules*** directory and used `cat` to put the contents of all rule files in the new ***suricata.rules*** file in */var/lib/suricata/rules/* directory.

<!-- TODO: Complete rule setup documentation - describe how rules were extracted, tested, and activated -->
