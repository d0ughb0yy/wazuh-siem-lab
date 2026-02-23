## Overview

This document covers the initial server hardening and Wazuh SIEM configuration for the SIEM server.

## Initial Configuration

### SSH Hardening

The `sshd_config` file in `/etc/ssh/` is configured to accept SSH connections only if the public key is present in the `authorized_keys` file of the `siem-admin` user.

```bash
PubkeyAuthentication yes
PasswordAuthentication no
```

- These two lines must be uncommented, and `PasswordAuthentication` set to `no`

For Ubuntu servers, an additional SSH configuration file exists in `/etc/ssh/sshd_config.d/`. This file was removed as it contained:

```bash
PasswordAuthentication yes
```

## Wazuh Hardening

A custom `siem-admin` user is created to replace the default `admin` user for improved security.

### Creating the Administrator User

1. Navigate to **Indexer Management** → **Security** → **Internal Users**
2. Create a new internal user named `siem-admin`

### Assigning Permissions

1. Navigate to **Indexer Management** → **Security** → **Roles**
2. Locate the `all_access` role and duplicate it
3. Name the duplicate role `siem_admin_access`
4. Edit the role settings and map the `siem-admin` user under the **Map User** tab

### Activating the User

1. Navigate to **Server Management** → **Security** → **Roles Mapping**
2. Create a new role mapping named `siem-admin-access`
3. Select the `administrator` role
4. Map it to the `siem-admin` user

Finally, restart the server to apply all changes.

## Next Steps

Continue to [Client Server Configuration](client-server-configuration.md) to set up Suricata and the Wazuh agent.
