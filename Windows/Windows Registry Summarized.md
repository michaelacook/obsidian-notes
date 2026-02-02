
[Thio Joe - What Are Those Different HKEY Registry Things in Windows Anyway?](https://www.youtube.com/watch?v=6ky_6kUJebY)
[Online Tech Tips - Windows Registry Explained](https://www.youtube.com/watch?v=KePKZTk8_7Y)
### Core Concepts

- The Windows Registry is made up of:
    - **Keys** (left side in Registry Editor)
    - **Values** (right side; the actual settings/data stored in keys)
- The top-level **HKEY_*** entries are called **Root Keys**
    - They are **logical/virtual starting points**, not actual registry files
- Registry was introduced with Windows 95. Prior to the registry, configuration was spread out across text files
- Types of registry values:
	- **REG_BINARY** - stores raw binary data
	- **REG_DWORD** - variable length 32-bit integer
	- **DWORD** - defines parameters for settings, drivers and apps
	- **REG_SZ** - fixed length string value, used for env variables
	- **REG_EXPAND_SZ** - expandable-length string value, used for env variables
	- **REG_MULTI_SZ** - array string containing a list of values normally separated by commas or spaces
	- **REG_RESOURCE_LIST** - list of resources in a nested array
	- **REG_RESOURCE_REQUIREMENTS_LIST** - array list of hardware resources used by drivers
	- **REG_FULL_RESOURCE_DESCRIPTOR** - nested arrays that store the resource list for physical hardware devices
	- **REG_LINK** - symbolic link (UNICODE) to another registry key, specifies both the root key and the path to the target key
	- **REG_NONE** - data that do not have a specific type
	- **REG_QWORD** - variable length 64-bit parameter (only available in 64-bit Windows)
- .reg files can be opened in Notepad and viewed in plain text, modified, and then re-imported

### Registry Storage (Where the Registry “Lives”)

- Registry data is stored across multiple files on disk
- Main system registry hive files are stored in `C:\Windows\System32\config\`:
    - `SYSTEM`, `SAM`, `SECURITY`, `SOFTWARE`
- These files mostly map to subkeys under:
    - **HKEY_LOCAL_MACHINE (HKLM)**
- User registry data is stored separately in the user profile as a **.DAT hive**, including:
    - **NTUSER.DAT**
    - **UsrClass.DAT**

### Meaning of “HKEY”

- **HKEY** = **Handle to Registry Key**
- Common abbreviations:
    - **HKCR** = HKEY_CLASSES_ROOT
    - **HKLM** = HKEY_LOCAL_MACHINE
    - **HKCU** = HKEY_CURRENT_USER

---

## Purpose of Each Root Key

### HKEY_CLASSES_ROOT (HKCR)

- Stores information about **registered applications**, including:
    - **File associations** (what program opens a file type)
    - **Object linking / COM info** (Class IDs / handlers)
- HKCR is a **merged view** of:
    - Default system settings:
        - `HKEY_LOCAL_MACHINE\Software\Classes`
    - User-specific overrides:
        - `HKEY_CURRENT_USER\Software\Classes`
- User settings override defaults in the combined HKCR view

### HKEY_LOCAL_MACHINE (HKLM)

- Contains **computer-wide settings** for the local machine, such as:
    - Installed hardware/software
    - Drivers and devices
    - System configuration data
    - Startup control parameters
- Many hive files in `C:\Windows\System32\config` correspond to HKLM subkeys

### HKEY_CURRENT_CONFIG (HKCC)

- Contains info about the system’s **current hardware profile**
- This root key is essentially an **alias** to:
    - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Hardware Profiles\Current`
- Shows only what differs from the default configuration

### HKEY_USERS (HKU)

- Contains registry settings for **all user profiles** on the machine
- Each user appears as a separate subkey
- Default profile template for new users is stored in:
    - `config\default`
- User-specific registry hives are stored in each user profile, mainly:
    - **NTUSER.DAT**

### HKEY_CURRENT_USER (HKCU)

- Contains registry settings for the **currently logged-in user**
- It is essentially a **pointer/alias** to that user’s entry under:
    - **HKEY_USERS**

### HKEY_PERFORMANCE_DATA

- Not normally visible in Registry Editor
- Does **not** have a file on disk
- A **virtual registry key** containing system performance information from:
    - Kernel
    - Drivers
    - Applications
- Performance tools (like Task Manager) read from this data
- Still accessed like a normal registry key through registry APIs