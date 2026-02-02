# Snapshots, Cloning, and Migration

Proxmox VE offers snapshotting, cloning and migration features to help administrators test changes, duplicate workloads and balance resources across nodes.  These tasks are typically performed via the `qm` CLI for virtual machines and `pct` for containers.

## Creating and managing snapshots

Snapshots capture the current state of a virtual machine, including memory, disk and device settings.  They allow you to roll back changes if an upgrade or configuration change fails.

* **Create a snapshot**:

  ```bash
  qm snapshot <vmid> <snapshotname>
  ```

  This command creates a snapshot of the VM with the given name【848419861440833†L527-L537】.  You can include spaces in the snapshot name by quoting it.

* **Rollback to a snapshot**:

  ```bash
  qm rollback <vmid> <snapshotname>
  ```

  The rollback command restores the VM to the specified snapshot and discards changes made after it was taken【848419861440833†L540-L550】.  Use rollback carefully because data written since the snapshot will be lost.

* **Delete a snapshot**:

  Use `qm delsnapshot <vmid> <snapshotname>` to delete a snapshot when it’s no longer needed.  Deleting snapshots frees storage space.

Snapshots require thin‑provisioned storage (LVM‑thin, ZFS or Ceph) to avoid copying entire disks.  Containers support snapshots via `pct snapshot` and `pct rollback`.

## Cloning virtual machines

Cloning copies an existing VM to a new VM ID.  You can perform full clones (copy all data) or linked clones (use the same base disk if supported by storage backend).

```bash
qm clone <vmid> <newid> [--name <newname>] [--full]
```

For example, `qm clone 103 104 --name "DebianClone" --full` creates a full clone of VM 103 named **DebianClone**【848419861440833†L554-L566】.  Linked clones can only be used on storage backends that support snapshots (e.g., LVM‑thin, ZFS, Ceph) and require less space.

## Migrating virtual machines

Migration moves a running or stopped VM from one Proxmox node to another.  Live migration requires shared storage or a Ceph pool accessible by both nodes.

```bash
qm migrate <vmid> <targetnode>
```

For example, `qm migrate 103 node2` migrates VM 103 to node2【848419861440833†L516-L525】.  If the VM is running, Proxmox will perform a live migration when possible; otherwise, it will fallback to offline migration.  Ensure that the VM’s disks reside on shared storage (e.g., Ceph, NFS) before initiating a live migration.  Migration is only supported within the same cluster and CPU compatibility (same vendor) is recommended【186625474225790†L388-L389】.

## Resizing disks

You can expand or shrink a virtual disk using:

```bash
qm resize <vmid> <disk> <size>
```

For example, `qm resize 103 scsi0 +10G` increases the `scsi0` disk by 10 GB【848419861440833†L568-L577】.  After resizing, expand the filesystem within the guest OS.  Shrinking disks requires caution and may not be supported by all filesystems.

## Containers

Containers have similar operations using the `pct` tool:

* `pct snapshot <ctid> <name>` and `pct rollback <ctid> <name>` for snapshots.
* `pct clone <ctid> <newid> [--full]` for cloning.
* `pct migrate <ctid> <targetnode>` for migration.  Containers require thin‑provisioned storage and shared storage for live migration.

## Best practices

* Take snapshots before performing system upgrades or major changes.  Limit the number of snapshots to reduce overhead.
* Use full clones when you need independent copies; use linked clones when conserving storage space.
* Migrate VMs during off‑peak hours if possible to minimize performance impact.  Monitor network throughput and CPU load during migration.

### Related notes
- [[Proxmox Overview]]
- [[Virtualization Architecture – KVM and QEMU]]
- [[Creating Virtual Machines]]
- [[Creating Containers]]
- [[Storage Types]]
- [[Clustering and pmxcfs]]
- [[Network Bridging and VLANs]]
- [[Command‑line Tools]]