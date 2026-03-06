# iSCSI Target & Hyper-V Initiator Setup Guide

## Overview

This guide covers creating virtual disks on a Windows Server iSCSI Target and connecting them from a Hyper-V host. This is commonly used for shared storage in Hyper-V clusters and live migration scenarios.

**Terminology**:

- **Target**: The server presenting (exporting) storage over the network
- **Initiator**: The client connecting to and consuming that storage
- **IQN**: iSCSI Qualified Name — unique identifier for targets and initiators
- **LUN**: Logical Unit Number — a virtual disk presented by the target

---

## Part 1: Configure the iSCSI Target Server

### 1.1 Install the iSCSI Target Role

Open PowerShell as Administrator:

```powershell
Install-WindowsFeature FS-iSCSITarget-Server -IncludeManagementTools
```

### 1.2 Create a Virtual Disk (VHD)

This creates the backing storage file that will be presented as a LUN.

**Via PowerShell:**

```powershell
# Create a 100GB fixed virtual disk
New-IscsiVirtualDisk -Path "E:\iSCSIDisks\Disk01.vhdx" -SizeBytes 100GB
```

> **Tip**: Use `-SizeBytes` for fixed provisioning. For thin provisioning (grows on demand), the default behaviour of `New-IscsiVirtualDisk` already creates a dynamically expanding VHDX.

### 1.3 Create an iSCSI Target

The target is the logical endpoint that initiators connect to.

```powershell
# Create a target and restrict access to specific initiator IQNs
New-IscsiServerTarget -TargetName "HyperV-Storage" `
    -InitiatorIds @("IQN:iqn.1991-05.com.microsoft:yourhost.domain.com")
```

> To find the initiator's IQN, run `(Get-InitiatorPort).NodeAddress` on the Hyper-V host.

### 1.4 Assign the Virtual Disk to the Target

```powershell
Add-IscsiVirtualDiskTargetMapping -TargetName "HyperV-Storage" `
    -Path "E:\iSCSIDisks\Disk01.vhdx"
```

### 1.5 Verify

```powershell
# List all targets
Get-IscsiServerTarget

# List all virtual disks
Get-IscsiVirtualDisk
```

---

## Part 2: Connect from the Hyper-V Host (Initiator)

### 2.1 Start the iSCSI Initiator Service

```powershell
Start-Service MSiSCSI
Set-Service MSiSCSI -StartupType Automatic
```

### 2.2 Discover the Target

```powershell
# Point the initiator at the target server's IP
New-IscsiTargetPortal -TargetPortalAddress "192.168.1.100"
```

### 2.3 Connect to the Target

```powershell
# List discovered targets
Get-IscsiTarget

# Connect (use -IsPersistent to survive reboots)
Connect-IscsiTarget -NodeAddress "iqn.1991-05.com.microsoft:yourserver-hyperv-storage" `
    -IsPersistent $true
```

> **For MPIO (multipath)**: If you have multiple network paths, install the MPIO feature first (`Install-WindowsFeature Multipath-IO`) and connect multiple sessions to the same target over different NICs.

### 2.4 Initialise and Format the Disk

Once connected, the iSCSI disk appears as a new raw disk in Windows.

```powershell
# Find the new disk (it will show as RAW and Offline)
Get-Disk | Where-Object { $_.OperationalStatus -eq "Offline" }

# Bring it online and initialise
$disk = Get-Disk | Where-Object { $_.OperationalStatus -eq "Offline" } | Select-Object -First 1
Set-Disk -Number $disk.Number -IsOffline $false
Initialize-Disk -Number $disk.Number -PartitionStyle GPT

# Create a partition and format
New-Partition -DiskNumber $disk.Number -UseMaximumSize -AssignDriveLetter |
    Format-Volume -FileSystem NTFS -NewFileSystemLabel "iSCSI-LUN01" -Confirm:$false
```

### 2.5 Verify the Connection

```powershell
# Check active sessions
Get-IscsiSession

# Check connection details
Get-IscsiConnection
```

---

## Part 3: Using iSCSI Storage with Hyper-V

Once the iSCSI LUN is mounted as a drive (e.g., `F:\`), you can:

- **Store VM files**: Point Hyper-V VM storage paths to the iSCSI drive
- **Use as a Cluster Shared Volume (CSV)**: In a failover cluster, add the disk as a CSV for live migration with shared storage
- **Live migration**: Both Hyper-V hosts connect to the same iSCSI target, so the VM's storage doesn't need to move — only memory and state transfer during migration

---

## Quick Reference

|Task|Command|
|---|---|
|Find initiator IQN|`(Get-InitiatorPort).NodeAddress`|
|List targets on server|`Get-IscsiServerTarget`|
|List virtual disks|`Get-IscsiVirtualDisk`|
|List discovered targets (initiator)|`Get-IscsiTarget`|
|Check active sessions|`Get-IscsiSession`|
|Disconnect a target|`Disconnect-IscsiTarget -NodeAddress "iqn..."`|
|Remove a virtual disk|`Remove-IscsiVirtualDisk -Path "E:\path.vhdx"`|

---

## Notes

- **Networking**: For production use, dedicate a separate VLAN/subnet for iSCSI traffic to avoid contention with management or VM traffic. Jumbo frames (MTU 9000) can improve throughput if configured end-to-end.
- **Security**: By default iSCSI has no encryption. Use CHAP authentication (`Set-IscsiServerTarget -EnableChap`) and network isolation for security.
- **Firewall**: The iSCSI target listens on TCP port **3260**. Ensure this is open between initiator and target.