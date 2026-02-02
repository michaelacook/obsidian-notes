# Backup and Restore

Proxmox VE provides both GUI and command‑line tools for backing up and restoring virtual machines (VMs) and containers.  Backups capture the state of a guest’s disks and configuration so you can restore it later or migrate it to another node.  Proxmox backups are typically stored in a designated storage pool (local directory, NFS, ZFS, or Ceph) as compressed archives.

## Built‑in backup jobs

In the web interface, backups are configured at **Datacenter → Backup**.  You can create schedules that run daily, weekly or monthly and select which guests to include.  Backups can be **full** or **incremental**, and you choose the storage target (e.g., local directory, NFS or Ceph)【91272477985482†L319-L406】.  During backup creation you can set retention policies, bandwidth limits and email notifications.  Proxmox uses **vzdump** in the background to perform the backups.

### Backup modes

The `vzdump` utility supports three modes【24966711610198†L70-L124】:

* **snapshot** (default) – uses LVM‑thin, ZFS or Ceph snapshots to create backups without stopping the VM or container.  Requires snapshot‑capable storage.
* **suspend** – briefly pauses the guest to create a consistent backup; downtime is minimal.
* **stop** – shuts down the guest, creates a backup and then restarts it.  Guarantees consistency at the cost of downtime.

Compression options include `gzip`, `lzo` and `zstd`; `zstd` offers a good balance between speed and compression ratio【24966711610198†L70-L124】.

### CLI backup example

To back up a VM from the command line, run:

```bash
vzdump <VMID> --mode stop --compress zstd --dumpdir /path/to/backup
```

The command above stops the VM, compresses the backup with Zstd and writes it to the specified directory【24966711610198†L70-L124】.  The resulting `.vma.zst` file contains the VM’s disk images and configuration.  You can run `vzdump --help` for more options.

## Restoring backups

### Restore via GUI

1. In the Proxmox web interface, select the node and then the **Backups** entry under the storage where the backup resides.
2. Choose the backup archive (identified by guest ID and timestamp) and click **Restore**.
3. In the restore dialog, select the target storage and optionally change the VM ID, limit network bandwidth or generate a unique MAC address.  Choose whether to start the guest after restore【91272477985482†L420-L466】.
4. Confirm to begin the restore.  Once complete, the restored VM appears in the inventory.【91272477985482†L420-L466】

### CLI restore example

On the command line, use `qmrestore` for VMs or `pct restore` for containers.  For example:

```bash
qmrestore /var/lib/vz/dump/vzdump-qemu-101-2024_07_01-03_00_00.vma.zst 201 --storage local-lvm
```

This restores the backup archive to a new VM with ID 201 on the `local-lvm` storage【24966711610198†L142-L156】.  If you restore over an existing VM ID, the old disks are replaced.  The CLI also supports options for bandwidth limiting and generating unique MAC addresses.

## Best practices

* Schedule regular backups for all critical guests; store backups on a different node or external storage to survive disk failures【91272477985482†L319-L406】.
* Use **snapshot** mode for VMs on thin‑provisioned storage (LVM‑thin, ZFS or Ceph) to avoid downtime; use **stop** mode for environments without snapshot support.
* Verify backups periodically by restoring them to a test VM.
* For cluster environments, store backups on shared storage (NFS, Ceph or Proxmox Backup Server) to enable restore on any node【162636434422459†L140-L144】.

### Related notes
- [[Proxmox Overview]]
- [[Storage Types]]
- [[Ceph Integration]]
- [[Creating Virtual Machines]]
- [[Creating Containers]]
- [[Snapshots, Cloning, and Migration]]
- [[Command‑line Tools]]