Windows RE is an extremely useful set of tools for repairing boot issues or performing troubleshooting that must be done outside of Windows. BitLocker some times removes Windows RE. This guide shows how to re-enable it and/or recreate the dedicated partition manually.

Useful reading:
- [Microsoft Learn: Windows Recovery Environment (Windows PE)](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference?view=windows-11)
- [Chris Titus: Reagentc Windows Recovery Partition](https://christitus.com/reagentc-windows-recovery-partition/)

### Enable and Disable WinRE

- `ReagentC /info` - See information about WinRE configuration
- `ReagentC /enable` - Enable WinRE
- `ReagentC /disable` - Disable WinRE

### Creating the Recovery Partition

BitLocker some times removes the recovery partition, so it needs to be recreated.
Steps:

1. Shrink the C drive by 600 MB to 1 GB (depending on available space)
2. Create a new simple volume with the uninitialized space
3. In diskpart:
	1. `format fs=ntfs quick label=WinRE`
	2. `set id="de94bba4-06d1-4d40-a16a-bfd50179d6ac"` on GPT or `set id=27` on MBR
	3. `assign letter=z` or another available letter
	4. `gpt attributes=0x8000000000000001`
4. Ensure the `winre.wim` file is in C:\Windows\System32\Recovery. If it isn't, extract it from a Windows ISO using 7zip or similar tool
5. Disable BitLocker on the Recovery drive with `Disable-BitLocker -MountPoint "Z:"` (if BitLocker is on)
6. Enable ReagentC with `ReagentC /enable`
7. Remove the drive letter in diskpart by selecting the volume and running `remove letter=Z`

If trying to enable WinRE with `ReagentC /enable` doesn't work, take the following steps (the partition will need a drive letter, so add it back if you removed it):

```ps
Z:
mkdir Recovery\WindowsRE
C:
cp C:\Windows\System32\Recovery\winre.vim Z:\Recovery\WindowsRE
reagentc /enable
```
