# Hyper-V PowerShell Command Reference

> ⚠️ **Run PowerShell as Administrator.** Do not run any of these scripts top-to-bottom — treat each block as a standalone reference. Replace `"WebServer01"` with your own VM name where applicable.
> 
> To enable the Hyper-V module if not already available:
> 
> ```powershell
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
> ```

---

## Table of Contents

- [[#1. Virtual Machine Management|1. Virtual Machine Management]]
- [[#2 Checkpoints (Snapshots)|2. Checkpoints (Snapshots)]]
- [[#3. Virtual Hard Disks (VHD/VHDX)|3. Virtual Hard Disks (VHD/VHDX)]]
- [[#4. Virtual Networking|4. Virtual Networking]]
- [[#5. VM Export & Import|5. VM Export and Import]]
- [[#6. Live Migration & Replication|6. Live Migration Replication]]
- [[#7. Hyper-V Host Management|7. Hyper-V Host Management]]
- [[#8. ISO / DVD Drives|8. ISO/DVD Drives]]
- [[#9. Secure Boot & TPM|9. Secure Boot & TPM)]]

---

## 1. Virtual Machine Management

```powershell
# Create a new VM
New-VM -Name "WebServer01" `
       -MemoryStartupBytes 2GB `
       -Generation 2 `
       -NewVHDPath "C:\Hyper-V\Disks\WebServer01.vhdx" `
       -NewVHDSizeBytes 60GB `
       -SwitchName "ExternalSwitch"

# Get a list of all VMs on the host
Get-VM

# Get detailed info about a specific VM
Get-VM -Name "WebServer01" | Format-List *

# Start, Stop, and Restart a VM
Start-VM   -Name "WebServer01"
Stop-VM    -Name "WebServer01"          # Graceful shutdown (requires integration services)
Stop-VM    -Name "WebServer01" -Force   # Hard power-off (like pulling the plug)
Restart-VM -Name "WebServer01"

# Suspend (pause) and Resume a VM
Suspend-VM -Name "WebServer01"
Resume-VM  -Name "WebServer01"

# Rename a VM
Rename-VM -Name "WebServer01" -NewName "WebServer01-PROD"

# Delete a VM (does NOT delete associated VHD files)
Remove-VM -Name "WebServer01" -Force

# Change VM startup memory
Set-VM -Name "WebServer01" -MemoryStartupBytes 4GB

# Enable Dynamic Memory with a min/max range
Set-VMMemory -VMName "WebServer01" `
             -DynamicMemoryEnabled $true `
             -MinimumBytes 512MB `
             -MaximumBytes 8GB `
             -StartupBytes 2GB

# Adjust virtual CPU count
Set-VMProcessor -VMName "WebServer01" -Count 4

# Disable automatic checkpoints
Set-VM -Name "WebServer01" -AutomaticCheckpointsEnabled $false

# Set VM to start automatically with the host (30 second delay)
Set-VM -Name "WebServer01" -AutomaticStartAction Start -AutomaticStartDelay 30

# Set VM action on host shutdown
Set-VM -Name "WebServer01" -AutomaticStopAction ShutDown
```

---

## 2. Checkpoints (Snapshots)

```powershell
# Create a checkpoint
Checkpoint-VM -Name "WebServer01" -SnapshotName "Pre-Update-2026-03-03"

# List all checkpoints for a VM
Get-VMCheckpoint -VMName "WebServer01"

# Restore (revert) to a specific checkpoint
$cp = Get-VMCheckpoint -VMName "WebServer01" -Name "Pre-Update-2026-03-03"
Restore-VMCheckpoint -VMCheckpoint $cp -Confirm:$false

# Rename a checkpoint
Rename-VMCheckpoint -VMName "WebServer01" `
                    -Name "Pre-Update-2026-03-03" `
                    -NewName "Clean-Baseline"

# Delete a checkpoint
Remove-VMCheckpoint -VMName "WebServer01" -Name "Clean-Baseline"
```

---

## 3. Virtual Hard Disks (VHD/VHDX)

```powershell
# Create a new dynamic VHDX disk
New-VHD -Path "C:\Hyper-V\Disks\DataDisk01.vhdx" `
        -SizeBytes 100GB `
        -Dynamic

# Create a fixed-size VHDX (better performance)
New-VHD -Path "C:\Hyper-V\Disks\DataDisk02.vhdx" `
        -SizeBytes 100GB `
        -Fixed

# Create a differencing disk (child disk based on a parent)
New-VHD -Path "C:\Hyper-V\Disks\Child.vhdx" `
        -ParentPath "C:\Hyper-V\Templates\BaseImage.vhdx" `
        -Differencing

# Attach a VHD to an existing VM
Add-VMHardDiskDrive -VMName "WebServer01" `
                    -Path "C:\Hyper-V\Disks\DataDisk01.vhdx"

# List all VHDs attached to a VM
Get-VMHardDiskDrive -VMName "WebServer01"

# Detach/Remove a VHD from a VM
Remove-VMHardDiskDrive -VMName "WebServer01" `
                       -ControllerType SCSI `
                       -ControllerNumber 0 `
                       -ControllerLocation 1

# Get info about a VHD file (size, type, etc.)
Get-VHD -Path "C:\Hyper-V\Disks\DataDisk01.vhdx"

# Resize (expand) an existing VHD
Resize-VHD -Path "C:\Hyper-V\Disks\DataDisk01.vhdx" -SizeBytes 200GB

# Compact a dynamic VHD to reclaim unused space
# The VHD must be dismounted first
Mount-VHD   -Path "C:\Hyper-V\Disks\DataDisk01.vhdx" -ReadOnly
Dismount-VHD -Path "C:\Hyper-V\Disks\DataDisk01.vhdx"
Optimize-VHD -Path "C:\Hyper-V\Disks\DataDisk01.vhdx" -Mode Full

# Convert a VHD to VHDX (or dynamic to fixed)
Convert-VHD -Path "C:\Hyper-V\Disks\OldDisk.vhd" `
            -DestinationPath "C:\Hyper-V\Disks\NewDisk.vhdx" `
            -VHDType Dynamic

# Merge a differencing disk into its parent
Merge-VHD -Path "C:\Hyper-V\Disks\Child.vhdx" `
          -DestinationPath "C:\Hyper-V\Disks\BaseImage.vhdx"
```

---

## 4. Virtual Networking

```powershell
# List all virtual switches
Get-VMSwitch

# Create an External switch (shares a physical NIC with the host)
New-VMSwitch -Name "ExternalSwitch" `
             -NetAdapterName "Ethernet" `
             -AllowManagementOS $true

# Create an Internal switch (host + VMs, no external access)
New-VMSwitch -Name "InternalSwitch" -SwitchType Internal

# Create a Private switch (VMs only, host cannot communicate)
New-VMSwitch -Name "PrivateSwitch" -SwitchType Private

# Delete a virtual switch
Remove-VMSwitch -Name "PrivateSwitch" -Force

# Get network adapters for a VM
Get-VMNetworkAdapter -VMName "WebServer01"

# Add a network adapter to a VM
Add-VMNetworkAdapter -VMName "WebServer01" -SwitchName "InternalSwitch" -Name "Management"

# Connect an existing adapter to a different switch
Connect-VMNetworkAdapter -VMName "WebServer01" `
                         -Name "Management" `
                         -SwitchName "ExternalSwitch"

# Disconnect a network adapter (without removing it)
Disconnect-VMNetworkAdapter -VMName "WebServer01" -Name "Management"

# Remove a network adapter from a VM
Remove-VMNetworkAdapter -VMName "WebServer01" -Name "Management"

# Set a static MAC address on a VM NIC
Set-VMNetworkAdapter -VMName "WebServer01" `
                     -Name "Network Adapter" `
                     -StaticMacAddress "00-15-5D-01-02-03"

# Enable VLAN tagging on a VM network adapter
Set-VMNetworkAdapterVlan -VMName "WebServer01" `
                         -Access `
                         -VlanId 100

# Limit VM network bandwidth to 100 Mbps
Set-VMNetworkAdapter -VMName "WebServer01" `
                     -MaximumBandwidth 100000000
```

---

## 5. VM Export & Import

```powershell
# Export a VM (stop it first for a consistent export)
Stop-VM -Name "WebServer01"
Export-VM -Name "WebServer01" -Path "D:\Hyper-V\Exports"

# Import a VM — Register mode (keeps original ID, in-place)
Import-VM -Path "D:\Hyper-V\Exports\WebServer01\Virtual Machines\*.vmcx" `
          -Register

# Import a VM — Copy mode (generates a new unique ID)
Import-VM -Path "D:\Hyper-V\Exports\WebServer01\Virtual Machines\*.vmcx" `
          -Copy `
          -VhdDestinationPath "C:\Hyper-V\Disks" `
          -VirtualMachinePath "C:\Hyper-V\VMs"

# Clone a VM by exporting then re-importing as a copy
Export-VM -Name "WebServer01" -Path "C:\Temp\Export"
Import-VM -Path "C:\Temp\Export\WebServer01\Virtual Machines\*.vmcx" `
          -Copy `
          -VhdDestinationPath "C:\Hyper-V\Disks" `
          -VirtualMachinePath "C:\Hyper-V\VMs" `
          -GenerateNewId

# Rename the newly cloned VM (it will be the most recently created)
Get-VM | Sort-Object CreationTime -Descending |
    Select-Object -First 1 |
    Rename-VM -NewName "WebServer01-Clone"
```

---

## 6. Live Migration & Replication

```powershell
# Move a running VM to another Hyper-V host (Live Migration)
Move-VM -Name "WebServer01" `
        -DestinationHost "HyperVHost02" `
        -IncludeStorage `
        -DestinationStoragePath "C:\Hyper-V\VMs"

# Move only VM storage while it keeps running
Move-VMStorage -VMName "WebServer01" -DestinationStoragePath "E:\Hyper-V\Disks"

# Enable Hyper-V Replica on the primary host
Set-VMReplication -VMName "WebServer01" `
                  -ReplicaServerName "ReplicaHost01" `
                  -ReplicaServerPort 80 `
                  -AuthenticationType Kerberos `
                  -RecoveryHistory 4 `
                  -ReplicationFrequencySec 300

# Start the initial replication seed
Start-VMInitialReplication -VMName "WebServer01"

# Check replication health and status
Get-VMReplication -VMName "WebServer01"
```

---

## 7. Hyper-V Host Management

```powershell
# Get current Hyper-V host settings
Get-VMHost

# Change the default VM and VHD storage paths on the host
Set-VMHost -VirtualMachinePath "D:\Hyper-V\VMs" `
           -VirtualHardDiskPath "D:\Hyper-V\Disks"

# Enable Enhanced Session Mode on the host
Set-VMHost -EnableEnhancedSessionMode $true

# Show CPU, memory, and uptime for all VMs in a table
Get-VM | Select-Object Name, State, CPUUsage, MemoryAssigned, Uptime |
    Format-Table -AutoSize

# List only VMs that are currently running
Get-VM | Where-Object { $_.State -eq "Running" }

# Get integration services status for a VM
Get-VMIntegrationService -VMName "WebServer01"

# Enable Guest Services (required for host-to-VM file copy)
Enable-VMIntegrationService -VMName "WebServer01" -Name "Guest Service Interface"
```

---

## 8. ISO / DVD Drives

```powershell
# Add a DVD drive to a VM and mount an ISO
Add-VMDvdDrive -VMName "WebServer01" -Path "C:\ISOs\Windows2022.iso"

# Swap the mounted ISO for a different one
Set-VMDvdDrive -VMName "WebServer01" -Path "C:\ISOs\Ubuntu-24.04.iso"

# Eject the ISO (leave the drive in place with no media)
Set-VMDvdDrive -VMName "WebServer01" -Path $null

# Remove the DVD drive entirely from the VM
Remove-VMDvdDrive -VMName "WebServer01" `
                  -ControllerNumber 0 `
                  -ControllerLocation 1
```

---

## 9. Secure Boot & TPM

> Applies to **Generation 2** VMs only.

```powershell
# Check current Secure Boot status and template
Get-VMFirmware -VMName "WebServer01" | Select-Object SecureBoot, SecureBootTemplate

# Disable Secure Boot (commonly needed for Linux guests)
Set-VMFirmware -VMName "WebServer01" -EnableSecureBoot Off

# Set the Secure Boot template for Linux guests
Set-VMFirmware -VMName "WebServer01" `
               -EnableSecureBoot On `
               -SecureBootTemplate MicrosoftUEFICertificateAuthority

# Change UEFI boot order to boot from DVD drive first
$dvd = Get-VMDvdDrive -VMName "WebServer01"
Set-VMFirmware -VMName "WebServer01" -FirstBootDevice $dvd

# Add a virtual TPM chip (required for Windows 11 guests)
# A local key protector must be set before enabling TPM
Set-VMKeyProtector -VMName "WebServer01" -NewLocalKeyProtector
Enable-VMTPM -VMName "WebServer01"
```