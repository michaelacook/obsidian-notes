- [New-VM Powershell Documentation](https://learn.microsoft.com/en-us/powershell/module/hyper-v/new-vm?view=windowsserver2025-ps)
- [Set-VM Powershell Documentation](https://learn.microsoft.com/en-us/powershell/module/hyper-v/set-vm?view=windowsserver2025-ps)
- Use `New-VM` cmdlet to create a VM
- Use `Get-VM` to see all vms
- Example of `New-VM` below:

```ps
New-VM -name "Name" `
  -MemoryStartupBytes 4GB `
  -Generation {1|2} `
  -NewVHDPath "C:\Path",
  -NewVHDSizeBytes 100GB
```

- Configure VM settings with `Set-VM`:

`Set-VM -Name "Name" -ProcessorCount 4`

- Attach an ISO image to the VM:

`Add-VMDvdDrive -VMName "name" -Path "Path"`

- Check boot order:

`Get-VM -Name "name" | Get-VMFirmware | Select BootOrder`

- Set the boot order in the vm firmware:

`set-VMFirmware -VMName "name" -FirstBootDevice "path\to\bootimage"`

- Start a vm:

`Start-VM -Name "name"`

- Connect to a vm:

`vmconnect.exe localhost "vmname"`

