# Optimizing VHDs in Hyper-V – PowerShell Reference Guide

---

## 1. Inspect VHD/VHDX Information

Before optimizing, understand what you're working with.

```powershell
# Get details on a single VHD
Get-VHD -Path "C:\VMs\MyVM\disk.vhdx"

# Get VHDs for a specific VM
$vm = "MyVM"
Get-VMHardDiskDrive -VMName $vm | ForEach-Object {
    Get-VHD -Path $_.Path
}

# Key properties returned:
# VhdType       – Fixed, Dynamic, or Differencing
# FileSize      – Actual file size on host disk
# Size          – Maximum provisioned size (logical)
# MinimumSize   – Smallest the disk can be shrunk to
# FragmentationPercentage – How fragmented the VHD is
```

---

## 2. Compact a Dynamic VHD (Reclaim Unused Space)

Dynamic VHDs grow as data is written but **do not shrink automatically**. Compacting reclaims blocks that were freed when files were deleted inside the guest.

> ⚠️ **The VM must be shut down** before compacting, unless using a passthrough disk.

```powershell
# Compact a single VHDX
Optimize-VHD -Path "C:\VMs\MyVM\disk.vhdx" -Mode Full

# Mode options:
# Full       – Scans entire VHD. Slowest, most thorough.
# Quick      – Only processes pre-zeroed blocks. Faster.
# Retrim     – Issues TRIM to underlying storage. For SSDs.
```

### Compact All VHDs for a Specific VM

```powershell
$vmName = "MyVM"
$disks = Get-VMHardDiskDrive -VMName $vmName

foreach ($disk in $disks) {
    Write-Host "Compacting: $($disk.Path)"
    Optimize-VHD -Path $disk.Path -Mode Full
}
```

### Compact All Dynamic VHDs on the Host

```powershell
Get-VM | Get-VMHardDiskDrive | ForEach-Object {
    $vhd = Get-VHD -Path $_.Path
    if ($vhd.VhdType -eq "Dynamic") {
        Write-Host "Compacting $($vhd.Path) [$([math]::Round($vhd.FileSize/1GB,2)) GB]"
        Optimize-VHD -Path $vhd.Path -Mode Full
    }
}
```

---

## 3. Prepare the Guest OS Before Compacting

For `Optimize-VHD` to reclaim the most space, deleted blocks inside the guest should be zeroed out first. Do this **before** shutting down the VM.

**Inside the Guest VM (Windows):**

```powershell
# Zero out free space using SDelete (Sysinternals) – run inside guest
# Download SDelete first: https://learn.microsoft.com/sysinternals/downloads/sdelete
sdelete64.exe -z C:

# --- OR use built-in defrag with slab consolidation ---
Optimize-Volume -DriveLetter C -SlabConsolidate -Verbose
```

**Full Workflow:**

```powershell
# Step 1: Run inside the guest VM to zero free space
Optimize-Volume -DriveLetter C -SlabConsolidate -Verbose

# Step 2: Shut down the VM from the host
Stop-VM -Name "MyVM" -Force

# Step 3: Compact the VHDX from the host
Optimize-VHD -Path "C:\VMs\MyVM\disk.vhdx" -Mode Full

# Step 4: Start the VM again
Start-VM -Name "MyVM"
```

---

## 4. Convert Between VHD Types

### Convert Dynamic → Fixed (for performance)

Fixed VHDs allocate all space upfront, reducing fragmentation and improving I/O predictability.

```powershell
Convert-VHD -Path "C:\VMs\MyVM\dynamic-disk.vhdx" `
            -DestinationPath "C:\VMs\MyVM\fixed-disk.vhdx" `
            -VHDType Fixed
```

### Convert Fixed → Dynamic (to save space)

```powershell
Convert-VHD -Path "C:\VMs\MyVM\fixed-disk.vhdx" `
            -DestinationPath "C:\VMs\MyVM\dynamic-disk.vhdx" `
            -VHDType Dynamic
```

### Convert Legacy VHD → VHDX

VHDX supports larger disks, better resilience, and improved performance.

```powershell
Convert-VHD -Path "C:\VMs\MyVM\legacy.vhd" `
            -DestinationPath "C:\VMs\MyVM\modern.vhdx"
```

---

## 5. Resize a VHD

### Expand a VHD

```powershell
# Resize to 200 GB
Resize-VHD -Path "C:\VMs\MyVM\disk.vhdx" -SizeBytes 200GB

# After resizing, extend the partition inside the guest:
# (Run inside the guest VM)
$disk = Get-Disk | Where-Object PartitionStyle -eq 'GPT' | Select-Object -Last 1
$partition = $disk | Get-Partition | Where-Object Type -eq 'Basic'
Resize-Partition -DiskNumber $disk.Number -PartitionNumber $partition.PartitionNumber -Size ($partition | Get-PartitionSupportedSize).SizeMax
```

### Shrink a VHD

You cannot shrink below `MinimumSize`. Always check first.

```powershell
$vhd = Get-VHD -Path "C:\VMs\MyVM\disk.vhdx"
$minSizeGB = [math]::Round($vhd.MinimumSize / 1GB, 2)
Write-Host "Minimum shrink size: $minSizeGB GB"

# Shrink to minimum (or set your own value above MinimumSize)
Resize-VHD -Path "C:\VMs\MyVM\disk.vhdx" -ToMinimumSize
```

---

## 6. Merge Differencing Disks (Checkpoint Cleanup)

Differencing disks (used by checkpoints/snapshots) chain off a parent VHD. Merging reduces chain depth and improves performance.

```powershell
# Check for checkpoints
Get-VMSnapshot -VMName "MyVM"

# Remove all checkpoints and merge (Hyper-V merges automatically on next shutdown)
Remove-VMSnapshot -VMName "MyVM" -IncludeAllChildSnapshots
Stop-VM -Name "MyVM"
# Hyper-V will merge the differencing disks during shutdown — monitor with:
Get-VM -Name "MyVM" | Select-Object Name, Status
```

---

## 7. Bulk Optimization Report

Generate a report of all VHDs on the host to identify candidates for optimization.

```powershell
$report = Get-VM | Get-VMHardDiskDrive | ForEach-Object {
    $vhd = Get-VHD -Path $_.Path
    [PSCustomObject]@{
        VM              = $_.VMName
        Path            = $vhd.Path
        Type            = $vhd.VhdType
        FileSizeGB      = [math]::Round($vhd.FileSize / 1GB, 2)
        MaxSizeGB       = [math]::Round($vhd.Size / 1GB, 2)
        MinSizeGB       = [math]::Round($vhd.MinimumSize / 1GB, 2)
        Fragmentation   = "$($vhd.FragmentationPercentage)%"
        WastedGB        = [math]::Round(($vhd.FileSize - $vhd.MinimumSize) / 1GB, 2)
    }
}

# Display in console
$report | Sort-Object WastedGB -Descending | Format-Table -AutoSize

# Export to CSV
$report | Export-Csv -Path "C:\Reports\VHD-Report.csv" -NoTypeInformation
```

---

## 8. Quick Reference Cheat Sheet

|Task|Cmdlet|
|---|---|
|Inspect a VHD|`Get-VHD -Path`|
|Compact / optimize|`Optimize-VHD -Path -Mode Full`|
|Convert type or format|`Convert-VHD -Path -DestinationPath -VHDType`|
|Expand a VHD|`Resize-VHD -Path -SizeBytes`|
|Shrink to minimum|`Resize-VHD -Path -ToMinimumSize`|
|List VM disks|`Get-VMHardDiskDrive -VMName`|
|List checkpoints|`Get-VMSnapshot -VMName`|
|Remove checkpoints|`Remove-VMSnapshot -VMName -IncludeAllChildSnapshots`|

---

## Tips & Best Practices

- Always **back up** VHDs before compacting, converting, or resizing.
- Run `Optimize-Volume` (slab consolidation) inside the guest **before** shutting down for maximum space reclaim.
- Prefer **VHDX over VHD** — better performance, larger size support, and corruption resilience.
- **Fixed VHDs** suit production workloads (predictable I/O); **Dynamic VHDs** suit dev/test environments.
- Fragmentation above **~10%** is a good trigger for optimization.
- Schedule compaction during off-hours — `Optimize-VHD -Mode Full` can be slow on large disks.