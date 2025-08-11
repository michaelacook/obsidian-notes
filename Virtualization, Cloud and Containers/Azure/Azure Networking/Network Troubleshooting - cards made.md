#### Azure Portal
- Monitoring tools to view network traffic stats
- Configuration info is useful for troubleshooting
- Effective routes provides built-in functionality to view the effective routes for a network interface card

#### Network Watcher
- provides advanced network troubleshooting tools as discussed in [[Azure Network Watcher - cards made]]
- must be enabled per subscription and per region
- tools
	- **Network Topology** - generates a diagram of a vnet
	- **Connection Monito**r - check connection between Azure resources
	- **Network Performance Monitor** - detects and alerts you to latency issues and pack drops
	- **IP Flow Verify** - checks if packets are allowed or denied for a specific virtual machine
	- **Next Hop** - determines how a packet gets from a VM to its destination
	- **Effective Security Rules** - displays the effective rules for a network interface
	- **Connection Troubleshoot** - records all of the packets sent to and from a VM
	- **VPN Troubleshoot** - diagnose VPN gateway connections