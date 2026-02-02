# Services and Daemons

Proxmox VE relies on a set of daemons that provide API access, status monitoring, high‑availability and cluster communication.  Understanding these processes helps when troubleshooting or automating the platform.

## pveproxy

`pveproxy` listens on port 8006 and serves the web interface and REST API over HTTPS.  It performs authentication and forwards requests to backend services.  When an API request targets another node, pveproxy automatically proxies the request, allowing administrators to manage an entire cluster from any node【961328732109269†L116-L133】.  Because it runs as an unprivileged user (`www‑data`), it forwards privileged operations to `pvedaemon`.

## pvedaemon

`pvedaemon` is the central management daemon for virtual machines, containers, storage and networking.  It handles privileged operations such as starting or stopping VMs/containers and modifying configurations.  In the architecture described by Veeam, pvedaemon is the main REST API server behind pveproxy【793481585297936†L247-L256】.

## pvestatd

`pvestatd` monitors the status of all resources (virtual machines, containers and storage) and distributes this information to cluster members【793481585297936†L258-L260】.  The daemon collects metrics such as CPU usage, memory consumption and disk utilization, enabling the GUI and CLI tools to display real‑time status.

## pve-ha-lrm and ha-manager

`pve-ha-lrm` is the **local resource manager** responsible for high‑availability (HA).  It monitors VMs and containers running on the local node and triggers recovery actions (e.g., restart or live migration) when a resource fails【793481585297936†L262-L266】.  The command‑line tool `ha-manager` performs cluster‑wide HA management and orchestrates failover across nodes【793481585297936†L316-L319】.

## pve-cluster and pmxcfs

`pve-cluster` manages cluster membership, quorum and inter‑node communication【793481585297936†L268-L272】.  It relies on the **pmxcfs** file system (see [[Clustering and pmxcfs]]) to replicate configuration files across nodes.  When a node joins or leaves a cluster, pve-cluster updates the cluster configuration and ensures a consistent view across members.

## Other services

* **pve-firewall** – provides cluster‑wide and per‑VM firewall management, using iptables or nftables and supporting rule sets at node and VM levels【793481585297936†L321-L326】.
* **pveceph** – a suite of tools for deploying and managing Ceph clusters within Proxmox【793481585297936†L311-L315】.  See [[Ceph Integration]].
* **pvesm** – storage management daemon integrated into the `pvesm` CLI tool for creating and managing storage pools【793481585297936†L294-L299】.  See [[Storage Types]].
* **pveum** – user management daemon behind the `pveum` tool for creating users and assigning roles【793481585297936†L304-L307】.  See [[User and Permission Management]].

### Related notes
- [[Proxmox Overview]]
- [[Clustering and pmxcfs]]
- [[Storage Types]]
- [[Ceph Integration]]
- [[User and Permission Management]]
- [[Command‑line Tools]]