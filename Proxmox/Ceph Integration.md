# Ceph Integration

**Ceph** is an open‑source distributed storage platform known for scalability, fault tolerance and support for block, object and file storage.  It stores data as objects on OSD (Object Storage Device) nodes and uses monitors (MON) to maintain cluster state【162636434422459†L30-L42】.  Ceph can run on commodity hardware and provides unified block, object and file interfaces.

## Why integrate Ceph with Proxmox

Proxmox VE offers native integration for installing and managing Ceph.  A Ceph‑backed Proxmox cluster brings several benefits:

* **Hyper‑converged infrastructure** – compute and storage resources reside on the same nodes, reducing hardware costs and simplifying management【162636434422459†L82-L99】.
* **High availability and scalability** – Ceph replicates data across multiple OSDs and nodes.  Adding more disks or nodes automatically increases capacity and resiliency【162636434422459†L95-L100】.  Combined with Proxmox’s HA stack, this delivers robust fault tolerance【162636434422459†L100-L102】.
* **Flexible storage pools** – administrators can create pools with different performance and redundancy characteristics (e.g., SSD pool for high‑I/O workloads, HDD pool for archives)【162636434422459†L100-L106】.
* **Simplified management** – Proxmox includes an installation wizard and the `pveceph` CLI to deploy, configure and monitor Ceph clusters directly from the web interface【162636434422459†L108-L126】.

## Basic setup workflow

1. **Install Ceph packages** via the Proxmox GUI or `pveceph install` on each node.  The wizard guides you through installation if Ceph is not yet installed【162636434422459†L108-L114】.
2. **Create cluster configuration** once per cluster.  Define a cluster name and specify the public network for Ceph traffic.  It is recommended to separate Ceph traffic from Proxmox management and guest networks to improve performance and security【162636434422459†L118-L121】.
3. **Add OSDs** by selecting unused disks via the web interface or `pveceph osd create`.  Ceph replicates data across OSDs to ensure redundancy【162636434422459†L122-L126】.
4. **Create storage pools**.  Pools define how data is distributed across OSDs.  Choose replication or erasure coding parameters that suit your workload.  Pools can be used for VM disks, container volumes or object storage【162636434422459†L126-L130】.

## Using Ceph in Proxmox

* **VM disks** – Ceph’s RADOS Block Device (RBD) interface can be used as a storage backend for VM images.  This enables live migration of VMs between nodes and increases fault tolerance【162636434422459†L134-L139】.
* **LXC containers** – container volumes can reside on Ceph pools, offering the same redundancy and performance benefits as VMs【162636434422459†L139-L141】.
* **Backup targets** – Proxmox’s built‑in backup tools can store backup archives on Ceph, leveraging its scalability【162636434422459†L140-L144】.
* **Object storage** – with Ceph’s Object Gateway, Proxmox clusters can provide an S3‑compatible object store for applications or archival data【162636434422459†L144-L148】.

Ceph requires at least three nodes for redundancy and separate networks for cluster and public communication.  When planning Ceph integration, ensure adequate network bandwidth (10 Gbit/s or higher is recommended) and consider SSDs for OSD journals to improve write performance【162636434422459†L152-L156】.

### Related notes
- [[Storage Types]]
- [[Proxmox Overview]]
- [[Clustering and pmxcfs]]
- [[Backup and Restore]]
- [[Creating Virtual Machines]]
- [[Creating Containers]]