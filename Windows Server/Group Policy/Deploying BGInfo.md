- Download BGinfo from Microsoft
- Save the executable and a template for the configuration to the scripts folder in SYSVOL
- Create a deployment script. Example:

```bat
\\ad.cooklab.com\SYSVOL\ad.cooklab.com\scripts\BGInfo\Bginfo.exe \\ad.cooklab.com\SYSVOL\ad.cooklab.com\scripts\BGInfo\bginfo_template.bgi /silent /nolicprompt /timer:0
```

- Deploy a GPO to send the script to Common Startup folder
	- New GPO, edit **User Configuration>Preferences>Windows Settings>Files**
	- Source: `\\ad.cooklab.com\SYSVOL\ad.cooklab.com\scripts\BGInfo\deploy-bginfo.bat`
	- Destination: `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\deploy-bginfo.bat`
- Link to OUs
- Note: if you have a GPO setting wallpaper, make sure it gets applied before this GPO
- Note: to find Common Startup folder open Run dialog and enter `shell:common startup`