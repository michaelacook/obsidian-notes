# Hyper-V VHD Sets – Quick Reference Guide

## What is a VHD Set?

A **VHD Set (.vhds)** is a shared virtual disk format introduced in Windows Server 2016, designed for **guest clusters** in Hyper-V. It replaces the older Shared VHDX approach, offering better backup and snapshot support without taking the disk offline.

A VHD Set consists of two files:

- **`.vhds`** — the metadata/descriptor file (small, contains disk configuration)
- **`.avhdx`** — the actual data file (the active leaf disk)

---

## Key Concepts

|Concept|Details|
|---|---|
|**Shared disk**|Multiple VMs in a guest cluster share the same `.vhds` disk simultaneously|
|**Checkpoints**|Supports online checkpoints without interrupting cluster workloads|
|**SAS requirement**|Requires a shared SCSI controller — cannot be attached to IDE|
|**Host requirement**|Windows Server 2016+ host; guest must be WS 2016+|
|**No live migration (solo)**|The VM(s) using the VHDS must all move together (shared nothing migration)|

---

## Common Use Cases

- **Windows Server Failover Clustering** — shared storage for clustered roles (SQL, File Server, etc.) without a physical SAN
- **Testing HA setups** in a lab environment
- **Hyper-V replica** scenarios requiring shared guest storage

---

## PowerShell Commands

### Create a new VHD Set

```powershell
New-VHD -Path "C:\VHDs\SharedDisk.vhds" -SizeBytes 50GB -Fixed
```

### Attach to a VM (must use shared SCSI controller)

```powershell
Add-VMHardDiskDrive -VMName "ClusterNode1" `
  -ControllerType SCSI `
  -Path "C:\VHDs\SharedDisk.vhds"
```

### Get information about a VHD Set

```powershell
Get-VHDSet -Path "C:\VHDs\SharedDisk.vhds"
```

### Get all checkpoints within a VHD Set

```powershell
Get-VHDSetSnapshot -Path "C:\VHDs\SharedDisk.vhds"
```

### Optimize (compact) a VHD Set

```powershell
Optimize-VHDSet -Path "C:\VHDs\SharedDisk.vhds"
```

### Remove a specific snapshot

```powershell
Remove-VHDSnapshot -Path "C:\VHDs\SharedDisk.vhds" `
  -SnapshotId "<GUID>"
```

---

## VHD Set vs. Shared VHDX

|Feature|Shared VHDX|VHD Set|
|---|---|---|
|Online backup/checkpoint|❌|✅|
|Resizing|Limited|✅|
|File format|`.vhdx`|`.vhds` + `.avhdx`|
|Min. host OS|WS 2012 R2|WS 2016|

---

## Tips

- Always store `.vhds` files on an **SMB 3.0 share** or **Cluster Shared Volume (CSV)** for proper shared access.
- The `.avhdx` data file is created automatically — do **not** manually move or delete it separately from the `.vhds`.
- Use `Get-VHDSet` to verify integrity after host failures.