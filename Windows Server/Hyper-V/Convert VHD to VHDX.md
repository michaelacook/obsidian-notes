# Converting VHD to VHDX – Quick Reference

## Rationale

VHDX supports larger disks (up to 64 TB vs 2 TB), has better resilience against corruption on unclean shutdowns, and supports larger block sizes for improved performance.

---

## Requirements

- The VM must be **powered off** (not saved, not paused — fully shut down)
- The VHD must **not be attached** to a running VM
- Enough free disk space for the new VHDX (roughly the same size as the source)
- Hyper-V role installed (Server/Client)

---

## Method 1: Hyper-V Manager (GUI)

1. Open **Hyper-V Manager**
2. In the right-hand **Actions** pane, click **Edit Disk…**
3. Browse to and select your `.vhd` file
4. Choose **Convert**
5. Select **VHDX** as the format
6. Choose disk type: **Fixed**, **Dynamically Expanding**, or **Differencing**
7. Specify the output path and filename (`.vhdx`)
8. Complete the wizard

> After conversion, go to the VM's **Settings → Hard Drive**, remove the old VHD, and attach the new VHDX.

---

## Method 2: PowerShell

```powershell
Convert-VHD -Path "C:\VMs\MyDisk.vhd" -DestinationPath "C:\VMs\MyDisk.vhdx" -VHDType Dynamic
```

**Common parameters:**

|Parameter|Values|Notes|
|---|---|---|
|`-VHDType`|`Dynamic`, `Fixed`|Omit to match source type|
|`-BlockSizeBytes`|e.g. `2MB`, `32MB`|Optional; default is usually fine|
|`-DeleteSource`|(switch)|Deletes original after conversion|

**Then swap the disk on the VM:**

```powershell
$vm = "MyVM"
$old = "C:\VMs\MyDisk.vhd"
$new = "C:\VMs\MyDisk.vhdx"

Remove-VMHardDiskDrive -VMName $vm -ControllerType SCSI -ControllerNumber 0 -ControllerLocation 0
Add-VMHardDiskDrive -VMName $vm -Path $new -ControllerType SCSI -ControllerNumber 0 -ControllerLocation 0
```

> Adjust controller type/number/location to match your VM's actual configuration. Check first with `Get-VMHardDiskDrive -VMName $vm`.

---

## Caveats

- **Differencing disks:** Convert the parent first, then recreate the differencing chain — you can't simply convert a differencing VHD in isolation.
- **Checkpoints:** Merge or delete all checkpoints before converting. The `.avhd`/`.avhdx` chain will not follow the conversion automatically.
- **Boot disk:** If converting the OS disk, confirm the VM boots successfully from the VHDX before deleting the original VHD.
- **Dynamic → Fixed (or vice versa):** You can change disk type during conversion, but Fixed disks pre-allocate all space immediately.
- **Guest-side prep (optional but recommended):** Before converting a dynamic disk, running `Optimize-VHD` or defragging inside the guest first can reduce wasted space in the output.