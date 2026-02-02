
## Legacy Win32 App Compatibility
- Before install: right-click executable, More Options, Troubleshoot compatibility
- After install: go to installed application location, find executable, select Properties>Compatibility
	- Select which compatibility option you want to run the app in
	- Change color mode, run as admin, use legacy color management, high DPI settings, etc.

## Repairing Installed Apps in Settings
- Restrict which kind of apps can be installed (Win32, Microsoft store) in Apps>Advanced app settings>Choose where to get apps
- Repair apps from Apps>Installed Apps

## Managing the Microsoft Store and Microsoft Store Apps
- Settings>Apps
- Manage app permissions, power, troubleshooting
- Apps>Advanced app settings>App execution aliases, App archive
- To reset Microsoft Store: go to app settings for Microsoft Store, reset
- With PowerShell:

```powershell
Get-AppXPackage *Microsoft.WindowsStore* |
Foreach {Add-AppxPackage -
DisableDevelopmentMode -Register
"$($_.InstallLocation)\AppXManifest.xml"
}

Get-AppxPackage -AllUsers *WindowsStore* |
Foreach {Add-AppxPackage -
DisableDevelopmentMode -Register
"$($_.InstallLocation)\AppXManifest.xml"
}

```

## Managing Browser Compatibility in Edge
- Apps>Optional features in older Windows 11 versions
- Exclusively in Edge in 25H2
- Edge>Settings>Default browser>Internet Explorer compatibility
- Local Group Policy: **Computer Config>Administrative Templates>Windows Components>Internet Explorer>Compatibility View**

## Testing Apps with Windows Sandbox
- Test apps in Windows Sandbox virtual machine
- Control Panel>Programs and Features>Turn Windows features on or off
	- Will require Hyper-V and Windows Hypervisor Platform features enabled as well
- Once activated, Windows Sandbox is available in the Start Menu
	- Minimal, testing environment
	- Nothing persists between sessions
		- Even if infected with malware, this will be erased when turning off

## Managing Processes with Scripting

- Processes:

```powershell
# Get a list of running processes on the PC
Get-Process

# Get data on a specific named process
Get-Process winword, explorer | Format-List *

# List processes based on their priority
$A = Get-Process
$A | Format-Table -View priority

# Get version information for a process
Get-Process pwsh -FileVersionInfo

# Find the owner of a specific process
Get-Process pwsh -IncludeUserName

# Find the window titles of running processes
Get-Process |
Where-Object { $_.MainWindowTitle } |
Format-Table Id, Name, MainWindowTitle -AutoSize

# Stop a named process
Stop-Process -Name <ProcessName> -Force

# Start a named process
Start-Process -FilePath "sort.exe"

```

- Services:

```powershell
# List services starting with "Net"
Get-Service "net*"

# Display services with a word in their descriptive name
Get-Service -DisplayName "*network*"

# Exclude terms from a service search
Get-Service -Name "net*" -Exclude "Netlogon"

# Obtain a list of services with a specific status
Get-Service |
Where-Object { $_.Status -eq "Running" -or $_.Status -eq "Stopped" -or $_.Status -eq "Suspended" }

# Stop a named service
Stop-Service -DisplayName <ServiceDisplayName> -Name <ServiceName>

# Start a named service
Start-Service -DisplayName <ServiceDisplayName> -Name <ServiceName>

# Suspend a named service
Suspend-Service -DisplayName <ServiceDisplayName> -Name <ServiceName>

# Restart a named service
Restart-Service -DisplayName <ServiceDisplayName> -Name <ServiceName>
```