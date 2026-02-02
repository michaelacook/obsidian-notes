# Clustering and pmxcfs

Proxmox VE allows multiple physical nodes to be combined into a **cluster**.  Clustering provides centralized management, live migration and high‑availability features.  The cornerstone of a Proxmox cluster is the **Proxmox Cluster File System (pmxcfs)**.

## pmxcfs

pmxcfs is a database‑driven file system used to store all Proxmox configuration files (VM definitions, storage definitions, user permissions, etc.).  It runs on top of **corosync** and replicates configuration data in real time to all nodes【186625474225790†L327-L330】.  The file system resides both on disk and in RAM; despite a size limit of 128 MiB, this is ample to store hundreds of VM configurations【186625474225790†L340-L343】.  Because every node has a consistent copy of the configuration, management tasks can be performed on any node (multi‑master architecture).

## Cluster manager (pvecm) and corosync

`pvecm` is the Proxmox cluster manager.  It uses the corosync cluster engine for reliable group communication.  There is no hard limit on the number of nodes in a cluster【186625474225790†L347-L351】.  You can use `pvecm` to create a new cluster, join or leave nodes, view status and perform other cluster operations【186625474225790†L347-L356】.  The corosync transport requires UDP ports 5405–5412 for communication【186625474225790†L378-L383】.

## Advantages of a cluster

Grouping nodes into a cluster provides several benefits【186625474225790†L360-L371】:

* **Centralized, web‑based management** – manage all nodes from a single interface.
* **Multi‑master architecture** – any node can perform management tasks.
* **Real‑time configuration replication** – pmxcfs ensures that configuration changes propagate instantly to all nodes【186625474225790†L327-L330】.
* **Easy migration** – virtual machines and containers can be moved between nodes without downtime.
* **Fast deployment** – adding nodes and configuring shared resources is streamlined.
* **Cluster‑wide services** – firewall rules, high‑availability policies and scheduled backups apply to all nodes【186625474225790†L360-L371】.

## Requirements for cluster formation

Before creating a cluster, ensure that:

* All nodes can communicate via UDP ports 5405–5412 for corosync【186625474225790†L378-L383】.
* System clocks are synchronized across nodes (e.g., using NTP)【186625474225790†L378-L383】.
* SSH connectivity on port 22 exists between nodes【186625474225790†L378-L383】.
* For high availability, at least three nodes are recommended to maintain quorum【186625474225790†L382-L384】.
* All nodes run the same Proxmox version【186625474225790†L382-L384】.
* A dedicated network interface is used for cluster traffic, separate from storage or migration networks【186625474225790†L385-L387】.
* The root password of the initial node is required when joining new nodes【186625474225790†L387-L389】.
* For seamless live migration, nodes should use CPUs from the same vendor, though it may work across vendors【186625474225790†L388-L389】.

## Creating a cluster

### Via the web interface

1. On the first node, navigate to **Datacenter → Cluster** and click **Create Cluster**.  Choose a unique cluster name and select the network interface (Link 0) used for cluster communication【186625474225790†L420-L423】.  Avoid using networks that carry heavy traffic such as storage or live migration【186625474225790†L424-L427】.
2. Click **Create** to form the cluster.  A progress indicator shows the deployment status【186625474225790†L433-L435】.

### Via CLI

Log into the first node via SSH and run:

```bash
pvecm create <CLUSTERNAME>
```

Check the cluster status with:

```bash
pvecm status
```

## Adding a node to the cluster

### Via the web interface

1. On the first node, go to **Datacenter → Cluster** and click **Join Information**.  Copy the join string, which includes the node’s IP and corosync credentials【186625474225790†L463-L469】.
2. On the node you want to add, navigate to **Datacenter → Cluster** and click **Join Cluster**.  Paste the join string, select the cluster network and enter the root password of the first node【186625474225790†L468-L484】.

### Via CLI

On the node you want to join, run:

```bash
pvecm add <IP_of_first_node>
```

The tool prompts for the root password and automatically fetches the cluster configuration.

## Removing a node

1. **Migrate or stop all workloads** on the node to be removed and ensure there are no replication tasks or local data requiring retention【186625474225790†L520-L523】.
2. On another node, run `pvecm nodes` to identify the name of the node to remove【186625474225790†L529-L533】.
3. Power off the node to remove【186625474225790†L538-L540】.
4. Run `pvecm delnode <nodename>` on a remaining cluster node to remove the offline node【186625474225790†L544-L549】.  You may see an error (`CS_ERR_NOT_EXIST`) because corosync cannot kill an offline node; this can be safely ignored【186625474225790†L549-L553】.
5. After removal, verify the cluster status using `pvecm status`【186625474225790†L554-L556】.

## Best practices

* Use a dedicated NIC or VLAN for corosync traffic to minimize latency and avoid packet loss【186625474225790†L424-L427】.
* Avoid changing hostnames or IP addresses after forming a cluster; reinstallation may be required to resolve misconfigured nodes【186625474225790†L393-L400】.
* Keep all nodes up to date to avoid SSL or protocol mismatches when joining new nodes【186625474225790†L488-L492】.

### Related notes
- [[Proxmox Overview]]
- [[Services and Daemons]]
- [[Storage Types]]
- [[Ceph Integration]]
- [[Creating Virtual Machines]]
- [[Creating Containers]]
- [[Backup and Restore]]
- [[Network Bridging and VLANs]]