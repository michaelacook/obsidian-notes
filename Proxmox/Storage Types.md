# Storage Types

Proxmox VE abstracts storage into two layers: **node‑level storage** and **datacenter‑level storage**.  Understanding the difference helps when provisioning disks for virtual machines, containers and backups.

## Node‑level storage

Physical storage devices (disks or partitions) are configured at the node level.  You can create local storage pools using **ZFS**, **LVM**, **LVM‑thin** or simple directories.  Once created, these pools can be assigned to the datacenter level for use by virtual machines and containers【51867353084062†L18-L34】.

## Datacenter‑level storage mapping

At the datacenter level, Proxmox maps storage pools to content types and cluster nodes.  A storage definition specifies what kinds of files it can hold—VM images, ISO templates, backup archives, container templates—and whether it is available on all nodes or just specific ones【51867353084062†L137-L149】.  This mapping separates how storage is managed (pool creation) from how it is used (VM disk, ISO store, backup target)【51867353084062†L40-L63】.  Administrators can add multiple storage backends and decide which nodes can access them, enabling shared or local storage configurations.

## Storage types

* **Directory** – a simple file‑level storage backed by a local filesystem (ext4 or xfs).  Useful for storing ISOs and backup files; it can also host VM disks but lacks snapshot support【51867353084062†L90-L114】.
* **LVM (Linear)** – uses Linux’s Logical Volume Manager to create logical volumes on physical disks.  Suitable for storing VM disks but does not support thin provisioning or snapshots.  You must allocate the entire disk space up front【51867353084062†L90-L114】.
* **LVM‑thin** – builds on LVM to provide thin provisioning, allowing VMs to consume space on demand rather than reserving the entire capacity.  It supports snapshots, making it a good choice for workloads requiring rollback【51867353084062†L90-L114】.
* **ZFS** – a combined filesystem and volume manager that offers advanced features like copy‑on‑write snapshots, compression, checksumming and built‑in RAID.  ZFS pools can be used directly to store VM disks and benefit from native snapshot and replication capabilities【51867353084062†L90-L114】.
* **Ceph RADOS Block Device (RBD)** – a distributed storage backend integrated into Proxmox via the **pveceph** tools.  Ceph provides highly available block storage for VM disks and containers, and scales horizontally by adding more OSDs (object storage devices).  See [[Ceph Integration]] for details【162636434422459†L82-L100】.

Other storage backends include NFS, iSCSI, CIFS (SMB), and GlusterFS, which can be added via the `pvesm` CLI or the GUI.  Each backend has unique performance and availability characteristics; Proxmox allows mixing different storage types within the same cluster.

### Tips

- Use **thin‑provisioned** storage (LVM‑thin or ZFS) when you need snapshots or want to over‑commit disk space; allocate disk space carefully on thin pools to avoid overfilling them.
- Directories are ideal for storing ISO files, container templates and backups because they do not require special management【51867353084062†L40-L63】.
- When using **ZFS**, dedicate a whole disk or partition to the pool and enable compression to save space.  Proxmox’s installer can set up a ZFS root with RAID configurations.
- **Ceph** requires at least three nodes (for redundancy) and separate networks for cluster and storage traffic.  It provides shared storage across the cluster for live migration and high availability【162636434422459†L108-L121】.

### Related notes
- [[Proxmox Overview]]
- [[Ceph Integration]]
- [[Creating Virtual Machines]]
- [[Creating Containers]]
- [[Backup and Restore]]
- [[Snapshots, Cloning, and Migration]]
- [[Command‑line Tools]]