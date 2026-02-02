## Windows Event Viewer
- Windows Tools>Event Viewer
- Summary of Administrative Events is most important panel under the Overview and Summary
![[Pasted image 20251204143500.png]]
- Can save log events as a .evtx file by right-clicking
- Can save a whole log using the right side Actions panel
- It is possible to schedule a task to occur on a certain event from the Event Viewer under the Actions menu, "Attach Task To This Event..."
- Create a custom view in the Actions menu

### PowerShell for Event Viewer
- `Get-EventLog -ComputerName <hostname> -LogName <logname>`
- Useful flags:
	- `-Newest <number>`
	- `Export-Csv -Path <path>`
	- `-EntryType <entry type>`
- Examples:

```powershell
Get-EventLog -LogName System -EntryType Error -Newest 10

Get-EventLog -LogName | WhereObject {$_.Eventid -eq 1074}

Get-EventLog -LogName System -Newest 10 | Export-Csv -Path "C:\systemlog.csv"

Get-EventLog -LogName System -Newest 10 | Select-Object -Property Index, timeGenerated, TimeWritten, EventType, Source, CategoryNumber, Message
```

## Resource Monitor
- Control Panel>System and Security>Windows Tools>Resource Monitor
- Useful for easily seeing CPU and processes, services, disk usage and TCP connections
- Probably easier to use than Task Manager for these things
- Not much to it, open Resource Monitor and view the information
- Can freeze monitoring to allow for easier viewing by clicking Monitor>Stop Monitoring