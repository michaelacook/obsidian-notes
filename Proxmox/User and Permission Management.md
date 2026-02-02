# User and Permission Management in Proxmox

Proxmox uses a **role‑based, path‑based permission system** to control who can access which objects.  Users are always associated with an authentication realm (such as Linux PAM or LDAP).  Roles define collections of privileges (e.g., the ability to power on VMs or change network settings) and are assigned on object paths (such as `/` for the whole datacenter or `/vms/101` for a specific VM) to users or groups.  Multiple authentication sources and strong two‑factor options make the system flexible and secure【887569295064917†L69-L75】【887569295064917†L86-L108】.

## Authentication realms

Authentication realms are configured in `/etc/pve/domains.cfg`.  Each realm defines how users are authenticated:

| Realm | Description |
|---|---|
| **Linux PAM** | Users exist on the host OS and have shell access.  Use PAM when you want Proxmox login credentials to mirror system accounts【887569295064917†L69-L75】. |
| **Proxmox VE (PVE)** | Internal authentication server; users are stored in `/etc/pve/priv/shadow.cfg`.  Suitable when you want Proxmox users that do **not** have shell access【887569295064917†L86-L108】. |
| **LDAP** | Integrates with an external directory service such as OpenLDAP【887569295064917†L96-L99】. |
| **Microsoft AD** | Uses Active Directory for authentication; supports LDAP protocol【887569295064917†L100-L103】. |
| **OpenID Connect** | Identity layer on top of OAuth 2.0; clients verify user identity via external provider【887569295064917†L100-L108】. |

## Two‑Factor Authentication

Proxmox supports multiple two‑factor methods (2FA).  You can enforce TOTP for all users in a realm or allow users to configure their own.  Supported methods include TOTP (Time‑based One‑Time Password), YubiKey OTP, WebAuthn (for FIDO/U2F security keys) and single‑use recovery keys.  Using 2FA is recommended when exposing the GUI over the internet【887569295064917†L114-L125】.

## Creating an administrative user

1. **Create a PAM user on the Debian host:** run `adduser <username>` on the host to create the account.  This user will become your new superuser.
2. **Add the user in the Proxmox GUI:** navigate to **Datacenter → Permissions → Users** and add the PAM user.  Assign the realm **PAM** and set a comment for clarity.
3. **Assign administrator privileges:** in **Datacenter → Permissions**, select the newly created user and grant the role **Administrator** at path `/`.  This gives full control over the cluster【887569295064917†L150-L166】.
4. **Disable root login:** after verifying that your new superuser works, expire the root password, disable SSH password logins for root and remove the root account’s ability to log in via the GUI【887569295064917†L150-L166】.

Proxmox cannot delete the `root@pam` account, but it can be disabled to improve security【887569295064917†L167-L167】.

## Adding other users and roles

After a superuser exists, you can add regular users.  Users can be assigned individually or grouped into **Groups** or **Pools** for easier management.  Proxmox ships with several predefined roles, including:

* **NoAccess** – denies all actions.
* **Administrator** – full privileges on all objects.
* **PVEAdmin** – administers the system but cannot create or remove users or groups.
* **PVEAuditor** – read‑only view across the cluster.
* **PVEVMAdmin/PVEVMUser** – manage or use VMs.
* **PVEDatastoreAdmin/PVEDatastoreUser** – manage or use storage.
* **PVEPoolAdmin/PVEPoolUser** – manage or use pools.
* **PVESDNAdmin/PVESDNUser** – manage or use Software‑Defined Networking.

You can create additional roles tailored to your organisation’s duties (e.g., granting only VM power control).  Roles are assigned via the `pveum` CLI or the **ACL** view in the GUI.  Granular permissions allow you to restrict access to specific nodes, VMs, disks or pools【887569295064917†L169-L183】.

## API tokens

For automation tools (e.g., Zabbix, monitoring systems), Proxmox provides **API tokens**.  Create a user (often with the `PVEAuditor` role) and then create a token for that user.  Tokens are shown only once; store them securely.  API tokens eliminate the need for password‑based authentication【887569295064917†L185-L190】.

## Related notes

* [[Graphical User Interface]] – explains how role‑based permissions integrate with the web interface.
* [[Command‑line Tools]] – covers the `pveum` CLI for managing users, groups and roles.
* [[Services and Daemons]] – describes the daemons that enforce authentication and permission checks across the cluster.
