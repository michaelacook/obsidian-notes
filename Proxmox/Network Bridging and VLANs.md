# Network Bridging and VLANs

Proxmox VE uses Linux network bridges to connect virtual machines and containers to physical networks.  A bridge acts like a virtual switch: guest interfaces attach to the bridge and the bridge connects to one or more physical network interface cards (NICs).

## Default bridge configuration

During installation, Proxmox creates a default bridge `vmbr0` that attaches to a physical NIC (e.g., `eth0`).  This bridge forwards traffic between the physical network and any guest connected to it.  A typical static configuration in `/etc/network/interfaces` looks like【985368148834176†L322-L343】:

```ini
auto lo
iface lo inet loopback

auto eno1
iface eno1 inet manual

auto vmbr0
iface vmbr0 inet static
    address 192.168.1.10/24
    gateway 192.168.1.1
    bridge-ports eno1
    bridge-stp off
    bridge-fd 0
```

`bridge-ports` lists the physical interfaces that form the bridge; you can specify multiple interfaces to provide NIC bonding or failover【985368148834176†L329-L343】.  `bridge-stp off` disables spanning tree protocol (enable it if bridging between multiple switches), and `bridge-fd 0` sets the forwarding delay to zero.

## DHCP configuration

If your environment uses DHCP, configure the bridge’s IP assignment accordingly:

```ini
auto vmbr0
iface vmbr0 inet dhcp
    bridge-ports eno1
    bridge-stp off
    bridge-fd 0
```

Guests connected to `vmbr0` will obtain IP addresses via DHCP from the external network【985368148834176†L360-L381】.

## VLAN‑aware bridges

Proxmox can create VLAN‑aware bridges to support multiple VLANs on a single bridge.  Add `bridge-vlan-aware yes` and `bridge-vids` to specify the VLAN IDs【985368148834176†L346-L359】:

```ini
auto vmbr0
iface vmbr0 inet static
    address 192.168.1.10/24
    gateway 192.168.1.1
    bridge-ports eno1
    bridge-stp off
    bridge-fd 0
    bridge-vlan-aware yes
    bridge-vids 2-4094
```

In this example, VLAN tags 2 through 4094 are allowed.  When configuring a VM or container, you can assign a VLAN tag (e.g., 10) to its interface; Proxmox will handle the tagging and pass traffic through the bridge.

## Tips

* Use separate bridges for different networks (e.g., management, storage, public) to isolate traffic.
* For clusters, dedicate a network interface or VLAN for corosync traffic (see [[Clustering and pmxcfs]]).
* VLAN‑aware bridges simplify configuration by eliminating the need to create separate Linux bridges for each VLAN.

### Related notes
- [[Proxmox Overview]]
- [[Creating Virtual Machines]]
- [[Creating Containers]]
- [[Clustering and pmxcfs]]
- [[Services and Daemons]]