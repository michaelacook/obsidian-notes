# cmdkey Reference Guide

## What It Is

`cmdkey` is a built-in Windows command-line utility for managing stored credentials in Windows Credential Manager. It lets you create, list, and delete saved usernames and passwords that Windows uses for automatic authentication to servers, shares, websites, and other resources.

Credentials managed by `cmdkey` are the same ones visible in **Control Panel > Credential Manager**.

## When You'd Use It

- Storing credentials for mapped network drives or UNC paths so users aren't prompted repeatedly
- Pre-staging credentials for scheduled tasks or scripts that connect to remote resources
- Troubleshooting authentication failures by listing or clearing stale/cached credentials
- Scripting credential cleanup during user offboarding or machine reprovisioning
- Managing RDP saved credentials (e.g. clearing an old password that keeps failing)
- Removing orphaned credentials after a server rename or decommission

## Credential Types

- **Generic** — App-level credentials (e.g. saved by Outlook, Teams, OneDrive, web browsers)
- **Domain** — Windows/NTLM/Kerberos credentials for network resources (shares, RDP, etc.)
- **Certificate-based** — Credentials backed by a certificate rather than a password

## Commands

### List All Stored Credentials

```
cmdkey /list
```

### List Credentials for a Specific Target

```
cmdkey /list:targetname
```

Example:

```
cmdkey /list:server01.hrd.local
```

### Add a Generic Credential

```
cmdkey /generic:targetname /user:username /pass:password
```

Example:

```
cmdkey /generic:app.example.com /user:mike@example.com /pass:P@ssw0rd!
```

### Add a Domain Credential

```
cmdkey /add:targetname /user:domain\username /pass:password
```

Example:

```
cmdkey /add:server01.hrd.local /user:hrd\mike /pass:P@ssw0rd!
```

> If you omit `/pass`, Windows will prompt for the password interactively — useful for scripts where you don't want plaintext passwords in the command.

### Delete a Credential

```
cmdkey /delete:targetname
```

Example:

```
cmdkey /delete:server01.hrd.local
```

### Add a Certificate-Based Credential

```
cmdkey /add:targetname /smartcard
```

## Target Name Formats

The target name is how Windows identifies what the credential is for. Common formats:

|Format|Example|Used For|
|---|---|---|
|Hostname|`server01`|SMB shares, RDP by short name|
|FQDN|`server01.hrd.local`|SMB/RDP by FQDN|
|UNC-style|`\\server01\share`|Specific share paths|
|TERMSRV/|`TERMSRV/server01`|RDP saved credentials|
|URL|`https://app.example.com`|Web/app credentials|

## Practical Examples

### Clear all saved RDP credentials for a specific server

```
cmdkey /delete:TERMSRV/server01.hrd.local
```

### Pre-stage credentials for a file share in a login script

```
cmdkey /add:fileserver.hrd.local /user:hrd\%USERNAME% /pass:%SHARE_PASS%
```

### Nuke a stale credential causing "access denied" after a password change

```
cmdkey /delete:server01.hrd.local
```

Then reconnect — Windows will prompt for fresh credentials.

### List everything to audit what's cached

```
cmdkey /list
```

## Things to Know

- `cmdkey` operates on the **current user's** credential store — it doesn't touch other users' saved credentials
- Credentials persist across reboots (stored in Credential Manager, not just session memory)
- Running `cmdkey` in an elevated prompt still targets the **calling user's** store, not the admin account's — unless you're actually logged in as that admin
- There is no built-in "clear all" command — you'd need to script a loop over `/list` output to delete everything
- For scripting bulk cleanup, PowerShell's `cmdkey /list` output can be parsed, or use the `CredentialManager` PowerShell module for a cleaner approach
- Stored passwords are encrypted with DPAPI and tied to the user profile — they don't survive profile migrations or credential exports without special handling