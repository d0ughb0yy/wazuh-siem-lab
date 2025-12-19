## Initial Configuration

### SSH Hardening

I edited the *sshd_config* file in */etc/ssh/* folder so that the server accepts connections via SSH **only** if the public key is in the *authorized_keys* file of the *siem-admin* user.

```
PubkeyAuthentication yes
PasswordAuthentication no
```
- These 2 lines need to be commented out and the yes changed to no on `PasswordAuthentication`

For the Ubuntu server there is an additional file for configuration located in  */etc/ssh/sshd_config.d/* directory which I deleted since it contained only:

```
PasswordAuthentication yes
```


## Wazuh Hardening

I decided to add a custom *siem-admin* user to the SIEM to prevent usage of the built-in *admin* user.

To do that I first had to create a new internal user located under
*Indexer Management* => *Security* => *Internal Users*.

I then created a new *siem-admin* internal user and moved to duplicate the *all_access* role and map the newly created user to the duplicated *all_access* role.
The setting is still under *Indexer Management* => *Security* but under *Roles*.
I located the *all_access* role and duplicated it naming it *siem-admin-access*.

To activate the user in Wazuh I switched to Server settings under:
*Server management* => *Security* => *Roles mapping*

From there I created a new role mapping and named it again *siem-admin-access* chose the role *administrator* and mapped it to my newly created *siem-admin* user.

At the end I restarted my server for the changes to take effect.