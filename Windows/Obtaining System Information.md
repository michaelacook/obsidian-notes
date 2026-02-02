## Reliability History
- Control Panel>System and Security>Security and Maintenance>Maintenance>View reliability history
- Shows a reliability score over time from 1 to 10
- Plots events from Event Viewer

## Windows Automatic Maintenance
- Control Panel>Security and Maintenance>Maintenance>Automatic Maintenance
- Automatic troubleshooters: System>Troubleshoot

## Blue Screen of Death
- Bluescreen View from Nirsoft

## Windows System Information
- Control Panel>System and Security>Windows Tools>System Information

## Retrieving System Info with PowerShell

| Category                        | Command                                                      | Description / Common Switches                                     |
| ------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------- |
| **System Info**                 | `Get-ComputerInfo`                                           | Full system information dump. Use `                               |
|                                 | `Get-CimInstance Win32_OperatingSystem`                      | OS name, version, architecture, uptime.                           |
|                                 | `Get-CimInstance Win32_Processor`                            | CPU name, core count, logical processors, max clock.              |
|                                 | `Get-CimInstance Win32_PhysicalMemory`                       | Memory module manufacturer, size, speed.                          |
|                                 | `Get-CimInstance Win32_OperatingSystem`                      | Select @{Name=...}`                                               |
|                                 | `Get-Disk`                                                   | Disk list; number, name, operational status.                      |
|                                 | `Get-PhysicalDisk`                                           | Disk hardware: size, media type (SSD/HDD), health.                |
|                                 | `Get-Volume`                                                 | Drive letters, filesystem, total/free space.                      |
|                                 | `Get-PSDrive -PSProvider FileSystem`                         | Mounted drive list.                                               |
| **Networking**                  | `Get-NetIPAddress`                                           | List all IP addresses assigned to interfaces.                     |
|                                 | `Get-NetIPConfiguration`                                     | IP + DNS + Gateway info (like ipconfig/all).                      |
|                                 | `Get-NetAdapter -Physical`                                   | Adapter name, status, link speed, MAC.                            |
|                                 | `Get-NetRoute`                                               | Routing table; use `                                              |
|                                 | `Get-NetFirewallRule -DisplayName "*Remote*"`                | Find firewall rules by name pattern.                              |
|                                 | `Test-Connection hostname -Count 4`                          | Ping equivalent with count option.                                |
|                                 | `Test-NetConnection -Port 443 hostname`                      | Tests port-level connectivity.                                    |
|                                 | `Resolve-DnsName domain.com`                                 | DNS lookup.                                                       |
|                                 | `Get-SmbShare`                                               | SMB shares hosted by this system.                                 |
|                                 | `Get-SmbMapping`                                             | SMB shares this system is connected to.                           |
| **Processes & Services**        | `Get-Process`                                                | All processes; use `                                              |
|                                 | `Get-Process`                                                | Select -First 10`                                                 |
|                                 | `Get-Service`                                                | All services; sort with: `                                        |
|                                 | `Get-Service -Status Running`                                | Only running services.                                            |
| **Event Logs**                  | `Get-WinEvent -LogName System -MaxEvents 50`                 | Latest 50 system log entries.                                     |
|                                 | `Get-WinEvent -FilterHashtable @{LogName="System"; Level=1}` | Critical system events only.                                      |
|                                 | `Get-WinEvent -LogName Application -MaxEvents 50`            | Recent application log entries.                                   |
| **Drivers & Updates**           | `Get-WmiObject Win32_PnPSignedDriver`                        | Device drivers with version and manufacturer.                     |
|                                 | `Get-WindowsUpdateLog`                                       | Generate Windows Update log.                                      |
|                                 | `Get-WindowsUpdate` _(requires PSWindowsUpdate module)_      | Shows available updates.                                          |
| **Security & Accounts**         | `Get-LocalUser`                                              | Local user accounts, enabled state, last logon.                   |
|                                 | `Get-LocalGroup`                                             | Shows groups.                                                     |
|                                 | `Get-LocalGroupMember -Group "Administrators"`               | List admin group members.                                         |
|                                 | `Get-NetFirewallProfile`                                     | Shows enabled/disabled firewall profiles.                         |
| **File & Lock Troubleshooting** | `OpenFiles.exe /Query`                                       | Lists open/locked files (must enable with `openfiles /local on`). |