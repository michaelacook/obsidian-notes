```powershell
########################################
# HYPER-V DIFFERENCING DISK SETUP
# Creates multiple VMs from a single
# parent (base) disk image.
#
# IMPORTANT: Before starting, prepare
# your parent disk:
# 1. Build and fully configure a VM
#    with all required base software
# 2. Sysprep the VM:
#    C:\Windows\System32\Sysprep\sysprep.exe
#    Select "Enter System Out-of-Box Experience"
#    Check "Generalize", then "Shutdown"
# 3. Do NOT boot the parent VM again after
#    Sysprep - it must remain in a clean,
#    shut-down state at all times.
########################################


# --- STEP 1 - Create a Differencing Disk ---
#
# Creates a new differencing disk (.vhdx) linked to a parent disk.
# The differencing disk stores only the CHANGES made from the parent,
# meaning the parent disk remains untouched.
# Each VM gets its own differencing disk - replace the paths to suit your environment.
# NOTE: Repeat this step for each VM you want to create.
New-VHD -Path "C:\VMs\Disks\VM01-diff.vhdx" -ParentPath "C:\VMs\Disks\Parent-Base.vhdx" -Differencing


# --- STEP 2 - Create a New VM Using the Differencing Disk ---
#
# Creates a new VM and assigns it the differencing disk created above.
# MemoryStartupBytes sets the RAM allocated at boot (2GB shown here).
# The VM will boot from the differencing disk, which already contains
# the fully configured parent OS - no reinstall needed.
# NOTE: Repeat this step for each VM, pointing to its own differencing disk.
New-VM -Name "VM01" `
       -MemoryStartupBytes 2GB `
       -VHDPath "C:\VMs\Disks\VM01-diff.vhdx" `
       -Generation 2


# --- STEP 3 - Connect the VM to a Virtual Switch ---
#
# Connects the VM's network adapter to an existing virtual switch.
# Replace "YourSwitchName" with the name of your switch.
# Run Get-VMSwitch to see available switches if unsure.
Connect-VMNetworkAdapter -VMName "VM01" -SwitchName "YourSwitchName"


# --- STEP 4 - Start the VM ---
#
# Powers on the VM. On first boot, Windows will run through its
# mini-setup (due to Sysprep) to assign a unique SID, hostname,
# and user account - ensuring each VM has its own identity despite
# sharing the same parent disk.
Start-VM -Name "VM01"


########################################
# REPEAT STEPS 1-4 FOR EACH ADDITIONAL
# VM, CHANGING THE DISK PATH AND VM NAME
# EACH TIME. FOR EXAMPLE:
#
# VM02:
# New-VHD -Path "C:\VMs\Disks\VM02-diff.vhdx" -ParentPath "C:\VMs\Disks\Parent-Base.vhdx" -Differencing
# New-VM -Name "VM02" -MemoryStartupBytes 2GB -VHDPath "C:\VMs\Disks\VM02-diff.vhdx" -Generation 2
# Connect-VMNetworkAdapter -VMName "VM02" -SwitchName "YourSwitchName"
# Start-VM -Name "VM02"
########################################


########################################
# USEFUL REFERENCE COMMANDS
#
# List all VHDs and their parent paths
# (confirms differencing chain is intact):
# Get-VHD -Path "C:\VMs\Disks\VM01-diff.vhdx"
#
# List all VMs and their current state:
# Get-VM
#
# List available virtual switches:
# Get-VMSwitch
########################################
```