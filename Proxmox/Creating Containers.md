# Creating Containers

This note outlines how to create an **LXC container** in Proxmox using the web interface.  Containers share the host kernel and provide lightweight isolated environments (see [[LXC Containers]]).

## Download a template

Before creating a container you need a template (rootfs tarball) for your desired distribution:

1. In the Proxmox GUI, navigate to **local (node) → CT Templates**.
2. Click **Templates** and select a Linux distribution (e.g., Debian, Ubuntu).  Click **Download** to fetch the template.

## Create the container

1. Click **Create CT**.  Provide a **CT ID**, **hostname** and **root password**【610956506847632†L190-L256】.
2. Choose the template you downloaded under the **Template** tab.【610956506847632†L190-L256】
3. **Disk** – select a storage pool and specify the disk size (thin‑provisioned pools such as LVM‑thin or ZFS support snapshots).  Containers typically require less storage than VMs.
4. **CPU** – allocate CPU cores and limit CPU weight or quota as required.
5. **Memory** – choose the memory limit; containers can share unused memory with the host.
6. **Network** – configure the container’s network interface.  Select a bridge (e.g., `vmbr0`), assign an IP address (static or DHCP) and specify a gateway.  You can also enable VLAN tagging if required【610956506847632†L190-L256】.
7. **DNS** – set DNS servers or use the host’s DNS settings.
8. Review the summary and click **Finish** to create the container.

Once created, the container appears in the tree.  You can start it and open a shell via the **Console** tab.  For CLI management, use the `pct` tool (see [[Command‑line Tools]]).

## Tips

* Use **unprivileged containers** for better security; this option is available in the **General** tab.
* Thin‑provisioned storage (LVM‑thin or ZFS) enables snapshots and efficient space usage.
* When using VLANs, configure **VLAN-aware bridges** in `/etc/network/interfaces`; see [[Network Bridging and VLANs]] for configuration examples.
* Resource limits can be adjusted after creation via the **Resources** tab or using `pct set`.

### Related notes
- [[Proxmox Overview]]
- [[LXC Containers]]
- [[Network Bridging and VLANs]]
- [[Storage Types]]
- [[Backup and Restore]]
- [[Snapshots, Cloning, and Migration]]
- [[Command‑line Tools]]