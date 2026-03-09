See enabled portals:

```powershell
C:\> Get-IscsiTargetServerSetting | Select-Object -ExpandProperty Portals

Enabled IPEndpoint
------- ----------
   True 10.0.3.1:3260
   True 10.0.3.2:3260
   True 10.0.3.3:3260
   True 10.0.3.4:3260
  False 192.168.1.252:3260
   True 192.168.127.91:3260
```

Disable a portal:

```powershell
# Only adapters on the 10.0.3.0/27 network should be used by the iSCSI target in this example, so we need to disable any others

Set-IscsiTargetServerSetting -IP 192.168.127.91 -Enable $false

Get-IscsiTargetServerSetting | Select-Object -ExpandProperty Portals

Enabled IPEndpoint
------- ----------
   True 10.0.3.1:3260
   True 10.0.3.2:3260
   True 10.0.3.3:3260
   True 10.0.3.4:3260
  False 192.168.1.252:3260
  False 192.168.127.91:3260
```

Do the same but with `$true` to enable a portal