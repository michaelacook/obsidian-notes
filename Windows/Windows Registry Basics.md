- Registry is the nervous system of Windows

## Registry Editor
- Windows Tools or Run>rededit.exe
- Always create a backup copy of the registry with File>Export
- Restore a backup with File>Import to import a .reg file
- Windows Recovery Console
	- Troubleshoot>Advanced Options>Command Prompt
		  - rededit.exe
		  - Once you're here, you're in the registry editor for the recover environment, not the boot system
			  - File>Import
				  - This PC (C:)>Windows
					  - Select a registry hive

## REG.EXE and REGINI.EXE
- For scripting registry changes
- reg.exe used with various switches, regini.exe uses plaintext scripts
- Also possible to script the registry with PowerShell

## Windows Registry
- Main Windows Registry files are in `%SystemRoot%\System32\config` which is usually in the C: drive
- Files used by Windows are
	- SAM (Security Accounts Manager)
	- SECURITY
	- SOFTWARE
	- SYSTEM
	- DEFAULT
	- USERDIFF (Used only for operating system upgrades)

### SAM
- Info about network domains the computer is connected to, including locally-created domain
- Corresponds to `HKEY_LOCAL_MACHINE\SAM\SAM`
- Each SAM database contains
	- username
	- Unique Identifier (UID) for the domain
	- cryptographic hash of the user password
	- location on the server of the user's Registry hive
	- other settings and flags
- Appear empty unless user is signed in with correct admin privileges

### SECURITY
- Refers to a critical system file that houses the **Security Registry Hive** for the Windows operating system. This file contains local security policies, user rights assignments, and audit settings.****
- Corresponds to `HKEY_LOCAL_MACHINE\SECURITY`
- local security policies for users and applications
- user rights
- system-wide security settings
	- user privileges, password policies, auditing settings
- Caches domain credentials locally so you can still access a domain profile while offline
- Appear empty unless user is signed in with correct admin privileges

### SYSTEM
- Info about Windows configuration
- Details for any currently mounted hardware devices, drivers and drives
- Boot access - loaded by Windows kernel during boot

### SOFTWARE
- Info about current Windows installation and all other installed software
- Keys organized by vendor name, include subkeys for file extensions, MIME types, Object Class, Interface IDs, and Active X controls


### `%UserProfile%\ntuser.dat`
- User-specific configuration for OS, apps and configuration
- Corresponds to `HKEY_CURRENT_USER`
- Critical user-specific registry file that stores personal settings, desktop configuration, and app preferences. It loads at login, stays locked during use, saves at logout, and should not be deleted.
- Used for restore points. When rolling back to a previous restore point, ensures account-specific configurations are rolled back as well

### `%LocalAppData%\Microsoft\usrclass.dat`
- UsrClass.dat is a per-user registry hive (part of the user profile)
- Stores user-specific COM/Class registrations and Explorer/Shell settings
	- User-specific file associations
- Helps Windows manage file associations, shell extensions, and related UI behavior
- Loaded when the user logs in and used during the session