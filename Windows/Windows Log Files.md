![[Pasted image 20251207134653.png]]

- Where to find Windows log files
- How to open and read log files
- Extracting useful information from log files

## Where to find logs

| Log          | Location                  | Purpose                                                                     |
| ------------ | ------------------------- | --------------------------------------------------------------------------- |
| PerfLogs     | C:\PerfLogs               | Created by Event Viewer                                                     |
| Debug        | C:\Windows\debug          | Plain text; various related to Windows Update, software, event logs         |
| Windows Logs | C:\Windows\Logs           | Readable in Event Viewer and some in Notepad; Windows Update and event logs |
| MiniDump     | C:\Windows\MiniDump       | Binary crash files, readable with BlueScreenView                            |
| Crash dump   | %LOCALAPPDATA%\CrashDumps | Binary crash files, readable with BlueScreenView; not always present        |
