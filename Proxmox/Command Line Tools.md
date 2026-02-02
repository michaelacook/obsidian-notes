# Command‑line Tools in Proxmox

While the [[Graphical User Interface]] provides a comprehensive way to manage the Proxmox platform, many tasks can also be automated or scripted using Proxmox’s command‑line tools.  The following sections introduce the most important utilities and their typical commands.

## `qm` – manage virtual machines

`qm` (short for *Qemu Machine*) is used to create, configure and control KVM virtual machines:

| Common command | Purpose |
|---|---|
| `qm start <vmid>` / `qm stop <vmid>` | Start or stop a VM. |
| `qm create <vmid> --name <name> --memory <size> --cores <n> --net0 <model=bridge>` | Create a new VM with the specified resources.  VM creation through `qm` is often scripted for automation. |
| `qm config <vmid>` | Show the current configuration for a VM. |
| `qm destroy <vmid>` | Remove a VM and its disks. |
| `qm snapshot <vmid> <snapname>` | Create a snapshot of a VM.  Snapshots require thin‑provisioned storage. |
| `qm rollback <vmid> <snapname>` | Roll back to a named snapshot. |
| `qm clone <vmid> <newid> --name <newname>` | Clone an existing VM, optionally as a linked clone or full clone. |
| `qm migrate <vmid> <target_node>` | Live migrate a VM to another node in the cluster. |
| `qm resize <vmid> <disk> <size>` | Resize a VM’s disk.  For example, `qm resize 100 scsi0 +10G` increases disk size by 10 GB【848419861440833†L520-L577】. |

These commands support additional options.  See the manual (`man qm`) or [[Creating Virtual Machines]] for more details.

## `pct` – manage containers

`pct` controls LXC containers.  It mirrors many `qm` functions but for containers:

| Command | Purpose |
|---|---|
| `pct start <ctid>` / `pct stop <ctid>` | Start or stop a container. |
| `pct create <ctid> <template> --rootfs <storage>:<size>` | Create a container from a template.  See [[Creating Containers]] for full steps. |
| `pct config <ctid>` | View or modify container configuration. |
| `pct destroy <ctid>` | Remove a container. |
| `pct resize <ctid> <disk> <size>` | Resize a container’s disk. |

Containers support snapshots via `pct snapshot` and `pct rollback`, though not all storage types support container snapshots.

## `pvesm` – storage management

`pvesm` is used to manage storage pools.  It allows administrators to list storage status, set what content a pool can hold and allocate volumes:

| Command | Description |
|---|---|
| `pvesm status` | List all configured storage and their status. |
| `pvesm set <storage-id> --content <types>` | Enable specific content types (e.g., images, iso, backup) on a storage pool【848419861440833†L720-L735】. |
| `pvesm alloc <storage-id> <vmid> <size>` | Allocate a raw volume for a VM or container【848419861440833†L726-L735】. |
| `pvesm add nfs <storage-id> --server <ip> --export <path> --content images,iso,backup` | Add an NFS share to the cluster (example). |

For more on storage types and configuration, refer to [[Storage Types]] and [[Ceph Integration]].

## `pveum` – user and permission management

The `pveum` utility manages users, groups, roles and ACLs.  It mirrors the functionality of the “Permissions” tab in the GUI.  Commands include:

* `pveum useradd <userid> --password <password> --comment <comment> --groups <group>` – create a user in the specified realm【848419861440833†L737-L770】.
* `pveum userdel <userid>` – delete a user【848419861440833†L772-L799】.
* `pveum groupadd <groupname> --comment <comment>` – create a group【848419861440833†L781-L794】.
* `pveum groupdel <groupname>` – delete a group【848419861440833†L796-L802】.
* `pveum roleadd <rolename> --privs <privileges>` – create a custom role with specified privileges【848419861440833†L803-L823】.
* `pveum rolemod <rolename> --privs <privileges>` – add or remove privileges from an existing role【848419861440833†L826-L839】.
* `pveum aclmod <path> --roles <role> --users <userid> --groups <group>` – assign a role to users/groups on a specific path【848419861440833†L841-L867】.
* `pveum realm list` – list configured authentication realms【848419861440833†L870-L894】.
* `pveum user list` – list all users【848419861440833†L897-L916】.

`pveum` is covered in more detail in [[User and Permission Management]].

## `pvesh` – REST API shell

`pvesh` provides an interactive shell for the Proxmox REST API.  It mirrors the web interface and supports listing resources, getting or setting properties and creating objects:

* `pvesh ls /path` – list available API endpoints, similar to `ls`【848419861440833†L935-L947】.
* `pvesh get /path` – retrieve information about a resource【848419861440833†L949-L962】.
* `pvesh set /path --key value …` – modify settings for a resource【848419861440833†L965-L980】.
* `pvesh create /path --key value …` – create a new resource, such as a VM【848419861440833†L982-L997】.
* `pvesh start /path` and `pvesh shutdown /path` – start or shut down a VM or container【848419861440833†L999-L1021】.
* `pvesh startall /nodes/<nodename>` / `pvesh stopall /nodes/<nodename>` – start or stop all VMs on a node【848419861440833†L1022-L1029】.

Using `pvesh` is helpful for automation and for exploring the Proxmox API.

## `pvecm` – cluster management

`pvecm` is the Proxmox Cluster Manager.  Use it to create and manage clusters:

| Command | Function |
|---|---|
| `pvecm create <clustername>` | Create a new cluster on the first node; this node becomes the master【848419861440833†L1047-L1060】. |
| `pvecm add <master-node-ip>` | Join the current node to an existing cluster【848419861440833†L1062-L1074】. |
| `pvecm delnode <nodename>` | Remove a node from the cluster【848419861440833†L1075-L1088】. |
| `pvecm nodes` | List all cluster nodes and their status【848419861440833†L1090-L1094】. |
| `pvecm setmaster <nodename>` | Promote a node to be the new master【848419861440833†L1096-L1103】. |
| `pvecm status` | Display cluster quorum and status information【848419861440833†L1104-L1109】. |
| `pvecm expected <number>` | Set the expected number of nodes for quorum【848419861440833†L1111-L1119】. |

Cluster requirements and best practices are discussed in [[Clustering and pmxcfs]].

## `ha-manager` – manage High Availability

`ha-manager` (sometimes spelled `ha-manager`) configures high‑availability for VMs and containers after a cluster is formed:

* `ha-manager status` – view the status of HA resources【848419861440833†L1123-L1141】.
* `ha-manager add <vmid> --group <groupname> --max-restarts <n> --max-migrate-tries <n>` – enable HA for a VM or container and set restart/migrate limits【848419861440833†L1143-L1168】.
* `ha-manager remove <vmid>` – remove a VM from HA management【848419861440833†L1170-L1179】.
* `ha-manager migrate <vmid> <targetnode>` – manually migrate an HA resource to another node【848419861440833†L1184-L1194】.

High‑availability concepts are described in [[Clustering and pmxcfs]].

## `vzdump` and `qmrestore` – backups and restores

`vzdump` creates backups of VMs or containers.  Backups can be full or incremental and may use compression.  The command accepts options such as `--mode` (snapshot/suspend/stop), `--compress` (lzo/gzip/zstd), `--storage` (target storage pool) and `--maxfiles` (retain only the latest `n` backups).  For example:

```
vzdump 101 --storage local --compress gzip --maxfiles 3
```

will back up VM 101 to the `local` storage, compressing it with gzip and keeping only the three most recent backups【848419861440833†L1196-L1258】.

To restore a backup, use `qmrestore <archive> <vmid> --storage <storage>`.  See [[Backup and Restore]] for GUI steps and additional options.

## Related notes

* [[Creating Virtual Machines]] and [[Creating Containers]] – for guided creation via GUI.
* [[Snapshots, Cloning, and Migration]] – deep dive into snapshots and migrations.
* [[User and Permission Management]] – to understand how roles and ACLs apply to users on the CLI.
* [[Clustering and pmxcfs]] – to learn about clusters and the `pvecm` tool.
