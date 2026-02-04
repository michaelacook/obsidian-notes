# Windows Server File Shares – Fundamentals

## Overview
- [Windows Server Storage documentation](https://learn.microsoft.com/en-us/windows-server/storage/storage)
- File sharing is one of the oldest and most basic server functions
- Purpose is centralized storage of files and folders shared with users over the network
- A folder is not accessible over the network until it is explicitly shared

## Basic Ways to Create a File Share

### File Explorer (End-User Style)
- Right-click folder → Give access to → Specific users
- Assign basic permissions (read or read-write)
- Intended for end users, not recommended for IT administration

### Advanced Sharing (IT/Admin Method)
- Right-click folder → Properties → Sharing → Advanced Sharing
- Enable sharing and configure share settings
- Change the share name
- Add a dollar sign ($) to create a hidden share
- Hidden shares require the full UNC path to access
- Limit the number of simultaneous users
- Configure offline caching
- Set share-level permissions

## Viewing and Managing Shares
- Use Computer Management → Shared Folders
- View all shared folders on the server
- See which users are connected
- See which files are open
- Disconnect users if necessary

## Server Manager File Sharing
- Requires the File and Storage Services role (File Server)
- Access via Server Manager → File and Storage Services
- View file servers, volumes, disks, and shares from one interface

## Creating Shares in Server Manager
- Use Tasks → New Share to launch the wizard

### Share Types
- SMB for standard Windows file sharing
- SMB Advanced requires File Server Resource Manager (FSRM)
- SMB for Applications optimized for application data such as Hyper-V or web apps
- NFS for Linux, Unix, and macOS environments

## SMB Protocol Basics
- Windows file sharing uses SMB (Server Message Block)
- SMB communicates over TCP port 445
- Linux and macOS can access SMB shares using Samba
- NFS is an alternative protocol mainly for non-Windows systems

## Common Share Options
- Access-Based Enumeration
  - Users only see files and folders they have permission to access
- Offline Caching
  - Allows offline access and later synchronization
  - Can cause data conflicts in multi-user shares
- Encryption
  - Encrypts data access over the network

## Permissions (High Level)
- Permissions can be set during share creation
- Users, groups, or computers can be assigned permissions
- When granting access to a server account, select the Computer object type
- Share permissions are separate from NTFS permissions

## Default Share Location
- Server Manager typically stores shares under a Shares folder
- Example path: `C:\Shares\FinanceData`

## PowerShell File Sharing
- Create shares using the `New-SmbShare` command
- The folder must exist before creating the share
- Default permission is read access for Everyone
- View existing shares using Get-SmbShare
- Hidden administrative shares such as C$ are visible to administrators

## Key Takeaways
- Folders must be explicitly shared to be accessible over the network
- Advanced Sharing and Server Manager are preferred administrative methods
- SMB is the primary Windows file-sharing protocol
- Server Manager provides centralized share management
- PowerShell enables automation and scripting for file shares

# BranchCache
- Since Windows 7
- Server 2008R2+
- Allow data to be cached on machines across the network
- Hosted cache mode uses a cache server, implemented by group policy
  - clients will check with hosted cache server first before going to more distant server
- Distributed cache mode means no servers, clients broadcast they are looking for a file and try to find it from a peer on the network
- To use, install BranchCache for Network Files in roles & features
