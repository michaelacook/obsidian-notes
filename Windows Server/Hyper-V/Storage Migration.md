# Hyper-V Storage Migration – Quick Reference Guide

---

## What Is Storage Migration?

Storage migration in Hyper-V allows you to move a VM's virtual hard disks (VHDs/VHDXs), configuration files, checkpoints, and smart paging files to a new location **while the VM remains running**. No downtime is required.

This is distinct from **Live Migration** (which moves a running VM between hosts) — storage migration moves the VM's data, not the VM itself across hosts.

---

## Core Concepts

- **Move types:** You can move the entire VM storage, or individual components (VHD only, config only, etc.)
- **Single host operation:** Storage migration moves data within or between storage locations accessible to the _same_ Hyper-V host.
- **Background copy:** Hyper-V mirrors the VHD to the destination while the VM runs, then performs a quick switchover.
- **Checkpoint awareness:** Active checkpoint chains are migrated along with the base disk — they cannot be left behind.

---

## Requirements

- The VM must be in a **Running, Saved, or Off** state (Paused is not supported).
- The destination path must be **accessible to the Hyper-V host** (local disk, SMB share, CSV, etc.).
- Sufficient **free space** at the destination (account for all VHDs + checkpoints + config files).
- For SMB destinations: the host computer account (not your user account) needs **read/write access** to the share.
- **No concurrent storage migrations** on the same VM — only one move operation at a time per VM.
- Hyper-V role must be installed; no clustering requirement for single-host storage migration.

---

## Caveats & Gotchas

- **Checkpoints slow things down.** Large checkpoint chains significantly increase migration time and required disk space.
- **Shared VHDs** (used in guest clusters) cannot be moved while the VM is running — the VM must be shut down.
- **Pass-through disks** cannot be migrated with this method.
- If the operation fails mid-way, Hyper-V cleans up the partial copy, but verify free space before retrying.
- Moving to an **SMB share** requires delegation to be configured if the host itself needs to access the share under a service account (double-hop scenario in clustered environments).
- This is **not a backup** — if the source disk is lost before migration completes, data is at risk.

---

## How To: Hyper-V Manager (GUI)

1. Open **Hyper-V Manager** and right-click the VM → **Move...**
2. Select **Move the virtual machine's storage**.
3. Choose a move option:
    - _Move all VM data to a single location_ — simplest option
    - _Move VM data to different locations_ — granular control per component
4. Specify the destination path(s).
5. Review the summary and click **Finish**.

---

## How To: PowerShell

```powershell
# Move all VM storage to a single destination folder
Move-VMStorage -VMName "MyVM" -DestinationStoragePath "D:\VMs\MyVM"

# Move only the VHD(s), specifying per-disk destination
Move-VMStorage -VMName "MyVM" -VHDs @(
    @{ SourceFilePath = "C:\VMs\MyVM\disk1.vhdx"; DestinationPath = "D:\VMs\MyVM" }
)

# Move config files and VHDs to separate locations
Move-VMStorage -VMName "MyVM" `
    -DestinationStoragePath "D:\VMs\MyVM" `
    -VirtualMachinePath "E:\VMConfigs\MyVM" `
    -SnapshotFilePath "E:\VMConfigs\MyVM\Snapshots"
```

---

## Quick Decision Guide

|Scenario|Approach|
|---|---|
|Move VMs to new storage, no downtime|Storage Migration (running VM)|
|VM has shared VHDs|Shut down first, then move|
|Move VM to a different host|Live Migration or Export/Import|
|Pass-through disk involved|Shut down, reassign disk manually|
|Large checkpoint chain present|Consider merging checkpoints first|