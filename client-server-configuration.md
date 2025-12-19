## Initial Configuration
### SSH Hardening
I again edited the *sshd_config* file in */etc/ssh/* folder so that the server accepts connections via SSH **only** if the public key is in the *authorized_keys* file of the *sys-admin* user this time.

### SMB Hardening
I created two different shares to simulate like a public *all employees allowed* share and a private only allowed for admin users.


**Global configuration:**
```
[global] 
	workgroup = WORKGROUP 
	string = Samba Server %v 
	netbios name = MY-SAMBA-SERVER 
	security = user
	map to guest = never
	log file = /var/log/samba/log.%m 
	max log size = 1000 
	server role = standalone server
```

**Private share configuration:**
```
[admin] 
	comment = Private Admin Share 
	path = /srv/samba/admin
	browsable = yes 
	guest ok = no ; No guest access 
	read only = no ; Writable after authentication 
	valid users = user1 user2 ; Replace with actual Samba usernames (add via: smbpasswd -a user1) 
	create mask = 0660 
	directory mask = 0770
```

**Public share configuration:**
```
[employees] 
comment = Public Employee Share 
path = /srv/samba/employees
browsable = yes 
guest ok = yes
read only = no
create mask = 0664 
directory mask = 0775 
force user = nobody
```

To create users for Samba, I used *smbpasswd* binary:
```
sudo smbpasswd -a sys-admin

sudo adduser siem-admin
sudo usermod -aG sudo siem-admin
sudo usermod -s /usr/sbin/nologin siem-admin
smbpasswd -a siem-admin
```
- Set the password after the prompt and the users for Samba is created
- For *siem-admin* user I set the *nologin* shell since I will use it to access SMB only

For directories I created them under */srv/samba/* and created 2 directories, one named admin which will be the private share and other named employees which will act as the public share.

Setup for the private share:
```Shell
sudo mkdir -p /srv/samba/admin
sudo groupadd smbprivate
sudo usermod -aG smbprivate sys-admin
sudo usermod -aG smbprivate siem-admin

sudo chgrp -R smbprivate /srv/samba/admin
sudo chmod -R 2770 /srv/samba/admin
```
- First I created the directory named *admin*
- Then I created a separate group for the 2 admin users which I will use to set permissions for the *admin* share
- At the end I assigned the ownership of the */srv/samba/admin* directory to the *smbprivate* group and set the permissions (The *2* indicates group permissions)

Setup for the public share:
```Shell
sudo mkdir -p /srv/samba/employees
sudo chmod -R 750 /srv/samba/employees
```

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
I replaced the ***eth0*** entries in */etc/suricata/suricata.yaml* file with my ***enp1s0*** interface, most important is to change under ***af-packet:*** section
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

Add the log format and the location of *eve.json* so the alerts can get sent to the SIEM server via Wazuh agent.

### Rule setup
I downloaded rules from [Emerging Threats website](https://rules.emergingthreats.net/open/suricata/) and downloaded the *emerging.rules.tar.gz* file containing all the rules I will need.

So I don't need to edit the default rule path I downloaded them to *sys-admin* home where I created a new ***suricata-rules*** directory and used `cat` to put the contents of all rule files in the new ***suricata.rules*** file in */var/lib/suricata/rules/* directory.