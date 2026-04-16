ws- Remote management of roles using tools like Server Manager requires WinRM to be enabled and running on both the local and remote machines
- Some management methods (e.g., MMC snap-ins using RPC/WMI) don’t strictly require WinRM, but modern Windows management (especially Server Manager) does
- To enable WinRM, open Terminal and run:

```ps
winrm quickconfig
```

- You may need to set the network profile to private or domain
- You may also need to add the remote host to the TrustedHosts list on the local (client)
	- This is a WinRM setting that specifies which remote hosts the computer is allowed to connect to when Kerberos isn't being used
	- Typically used in a workgroup environment
	- Without adding the remote host to the TrustedHosts list, WinRM in a workgroup (non-Kerberos) environment will fail

```ps
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "remote-hostname"

# Restart WinRM
Restart-Service winrm
```

- You will also likely need to add the credentials for the remote host in Windows Credential Manager

```ps
cmdkey /add:hostname /user:user /pass

# Enter password when prompted
```

- You should now be able to connect to the remote host using Server Manager or Hyper-V Manager