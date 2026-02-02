## System File Checker
- Run in an elevated command prompt
- `sfc /scannow`
- Checks all critical system files against it's database to determine if anything is missing, corrupted or changed
- Will attempt to restore correct file versions from it's backup or Windows Update

## Windows Update Branches
- Consumer Windows:
	- Can pause Windows updates for up to 5 weeks
	- Feature updates have 18 months of support
- Business:
	- Quality updates can be deferred for 30 days
	- Feature updates can be deferred for 1 year
	- Feature updates have 30 months oof support
	- Updates are managed by Windows Server Update Services or Configuration Manager
- Long-term Servicing Channel (specialized devices):
	- LTSC updates released every 2-3 years
	- Quality and Feature updates can be skipped for up to 10 years
	- Updates are managed by WSUS or Configuration Manager
	- Not intended for any PC where Office would be installed

## Managing Windows Update
- Advanced options in Windows Update
	- Many options you can configure, peruse it

## Uninstalling Windows Updates
- Windows Update>Update history
	- Displays a list of recently installed updates by category
- Uninstall updates brings you to Control Panel/settings where updates can be uninstalled similar to programs
- Can't uninstall important security & stability updates
- Can also run System Restore
	- System Restore>Create a restore point
	- If System Restore is turned on it will automatically create a restore point before major updates run
	- System Restore can be run from Windows Recovery if you can't boot the system 
		- Restart while holding down shift
- If an update is causing serious issues, after uninstalling it you can pause updates for a few weeks to give Microsoft a chance to fix whatever issue may be arising

## Resetting Windows Update
- Bad/corrupt updates occasionally get pushed out that won't install
- This requires resetting Windows Update
	- Services>Windows Update, Windows Update Medic Service, BITS
		- Stop services
	- File Explorer>`C:\Windows\Software Distribution`
		- Delete everything in the folder
			- If you can't delete everything, it's because Windows Update service started again
				- Stop it again, delete Windows Update cache quickly
					- Then retry Windows Update again
					- Maybe reboot first

## Managing Updates using PowerShell
- `Get-HotFix { ComputerName name, name, ..., name } | Sort-Object InstalledOn` <- Check what updates have been recently installed on PCs across the network
- `Get-WindowsUpdateLog` <- Exports the Windows Update log on a PC to a WindowsUpdate.txt file placed on the desktop

## Windows Reset
- Last resort
- On the desktop>Settings>System>Recovery
- Also available from Windows Recovery>Troubleshoot>Reset this PC

## Manually install update
- `wmic qfe get hotfixid` in CMD to confirm if update is actually installed
- `winver` to get version
- 