If you're trying to run certain commands against a remote device in the domain and get "RPC Server unavailable" then you need to enable DCOM/RPC firewall rules
- New GPO. `Computer Configuration>Windows Settings>Security Settings>Windows Defender Firewall with Advanced Security`
- Inbound Rules
- Select Predefined: Windows Management Instrumentation (WMI)
- Alternatively, you can enable to following predefined rules inbound on the target device:
	- Windows Management Instrumentation (ASync-In)
	- Windows Management Instrumentation (DCOM-In)
	- Windows Management Instrumentation (WMI-In)