# Graphical User Interface

Proxmox VE includes a comprehensive **web‑based GUI** built on the ExtJS 6.x framework.  The GUI is designed to manage entire clusters from a single browser window using modern web technologies such as AJAX and HTML5.  This note summarises its key features, layout and useful functions.

## Key features

* **Seamless cluster management:** The GUI integrates all nodes into a single view, enabling central management of virtual machines, containers, storage and networks.  It leverages AJAX for dynamic updates, so resource usage and status information refresh without reloading the page【187490465000875†L464-L469】.
* **Secure access:** Communication takes place over HTTPS using SSL encryption.  Both VMs and containers can be accessed via SPICE or an HTML5 console for remote display【187490465000875†L466-L472】.
* **Fast search and scaling:** The interface includes a search‑driven navigation capable of handling hundreds or thousands of VMs【187490465000875†L469-L470】.
* **Role‑based permissions:** Permissions are enforced through roles and ACLs.  Administrators can limit user actions on VMs, storage, nodes and other objects.  This system is described in [[User and Permission Management]].
* **Multi‑factor authentication:** Two‑factor methods such as YubiKey or TOTP can be enforced at the realm level【187490465000875†L462-L463】.
* **Multiple authentication sources:** The GUI supports local Proxmox authentication, PAM, LDAP, Active Directory and OpenID Connect.  Users can select their authentication realm and preferred language at login【187490465000875†L479-L483】.

## Layout overview

The Proxmox web interface is divided into four main sections【187490465000875†L493-L540】:

| Section | Description |
|---|---|
| **Header** | Located at the top of the page; displays server status and provides quick action buttons such as **Create VM**, **Create CT**, **Help**, and **Logout**【187490465000875†L501-L505】.  A gear icon opens *My Settings* for personal preferences. |
| **Resource Tree** | Left‑hand navigation tree used to browse objects.  Different views (Server, Storage, Pool, Folder) tailor the tree to specific needs【187490465000875†L510-L528】. |
| **Content Panel** | The central pane displays the configuration and status of the selected object【187490465000875†L531-L535】.  For example, selecting a VM shows its summary, hardware settings and console access. |
| **Log Panel** | Bottom section listing recent tasks and logs.  Double‑click entries to view details or abort running tasks【187490465000875†L536-L541】. |

### My Settings

The **My Settings** dialog (accessible via the gear icon) stores user‑specific preferences in the browser.  Users can reset saved login names, change the GUI layout, hide or show storage statistics in the datacenter summary and tweak the embedded xterm.js console (font family, size, spacing, line height)【187490465000875†L545-L568】.  These settings affect only the current browser and do not modify cluster configuration.

## Using the GUI

* **Login:** Navigate to `https://<proxmox-host>:8006` and enter your credentials.  Choose the appropriate authentication realm and language.  Optionally, save the username for convenience【187490465000875†L479-L489】.
* **Creating VMs/CTs:** Use the **Create VM** or **Create CT** buttons in the header.  This opens a multi‑step wizard to upload an ISO, assign resources and network settings.  Detailed instructions are provided in [[Creating Virtual Machines]] and [[Creating Containers]].
* **Monitoring:** The dashboard provides an overview of CPU, memory, storage and cluster health.  The log panel tracks tasks such as migrations, backups and snapshots.
* **Editing resources:** Selecting an object in the resource tree exposes tabs (Summary, Console, Hardware, Network, etc.) in the content panel.  These tabs mirror CLI commands like `qm config` and `pct config`.
* **Search:** Use the search field in the header to quickly locate VMs, containers or other objects by name or ID.

## Related notes

* [[User and Permission Management]] – covers roles, authentication realms and 2FA options in detail.
* [[Command‑line Tools]] – CLI alternatives to the GUI for automation and scripting.
* [[Clustering and pmxcfs]] – explains how the GUI manages multi‑node clusters under the hood.
