# Upgrading VM Configuration Version in Hyper-V

> **Prerequisites:** The VM must be **shut down** (not saved/paused). The upgrade is **irreversible** — the VM won't be openable on older Hyper-V hosts afterward.

---

## Hyper-V Manager

1. Shut down the VM.
2. Right-click the VM → **Upgrade Configuration Version...**
    - If this option is greyed out or missing, the VM is already at the latest version.
3. Confirm the prompt.

---

## PowerShell

**Check current version:**

```powershell
Get-VM -Name "VMName" | Select-Object Name, Version
```

**Check what version the host supports:**

```powershell
Get-VMHostSupportedVersion
```

**Upgrade a single VM (to latest supported by host):**

```powershell
Update-VMVersion -Name "VMName"
```

**Upgrade to a specific version:**

```powershell
Update-VMVersion -Name "VMName" -Version "9.0"
```

> Useful when you need compatibility with a specific host OS — e.g. upgrading to `9.0` (Server 2019) rather than the host's max if you still need portability to 2019 nodes.

**Upgrade all VMs on the host (use with caution):**

```powershell
Get-VM | Update-VMVersion -Force
```

> Omit `-Force` to be prompted per-VM for confirmation.

---

## Version Reference

|Config Version|Host OS|
|---|---|
|8.0|Windows Server 2016 / Windows 10|
|9.0|Windows Server 2019 / Windows 10 1809+|
|10.0|Windows Server 2022 / Windows 11|
|11.0|Windows Server 2025 / Windows 11 24H2|

> Always target the **lowest version** that gives you the features you need if VMs need to remain portable across hosts running different OS versions.

## Notes

- In a cluster, drain the node and use `Update-VMVersion` per VM after live migrating to the target host.
- Checkpoints taken before the upgrade may behave unexpectedly — apply or delete them first.