```powershell
# Download the WAC installer
Start-BitsTransfer -Source "https://aka.ms/wacdownload" -Destination "C:\Users\Administrator\Downloads"

# Install with defaults
cd C:\Users\Administrator\Downloads
Start-Process -FilePath .\wac.exe -ArgumentList /VERYSILENT -Wait
```

Once installed, you should be able to access Windows Admin Center from a browser on another machine