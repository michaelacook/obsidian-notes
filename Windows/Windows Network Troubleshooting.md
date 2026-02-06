## Network Settings and the Network and Sharing Center
- Settings>Network & internet
- Control Panel>Network and Internet
	- Change Adapter Settings
	- May eventually be completely folded into Settings

## Getting Information about Network Connections
- Run dialog, `ncpa.cpl `command opens Network Connections

## Configuring and Resetting Network Settings
- Reset network configuration when troubleshooting fails
- Two options:
	- **Settings>Network & internet>Advanced network settings>Network reset** or
	- Reset TCP/IP network stack in Command Prompt with `netsh int ip reset`

## Troubleshooting Networking with Microsoft Sysinternals
- Sysinternals tools for Active Directory
	- `ADExplorer` - View and edit AD database; copy the database; compare AD databases
	- `ADInsight` - Real-time monitoring tool for Active Directory, allowing you to troubleshoot client apps, view processes and events
	- `ADRestore` - Undeleted 'tombstoned' AD objects on a domain
- Other tools
	- `PsFile` - Provide info on files locked by access over the network
	- `PsInfo` - Displays info about a remote PC over the network
	- `PsPing` - More configurable version of ping, can test TCP latency and bandwidth
	- `PsKill` - Used to kill a running process on a PC over the network
	- `PsLoggedOn` - Displays details of which users are signed in to a remote PC
	- `PsShutdown` - Shutdown or force a restart on a remote PC
	- `TCPView` - Displays information about endpoint network connections from the PC, including IP of the destination and the port used
	- `TCPVcon` - Command line version of `TCPView`