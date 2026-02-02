## Users and Organizational Units
- How to organize OUs?
  - site locations
  - departments, divisions
  - object type like users, computers, groups
  - organize any way that makes sense for your environment
- Users (and other objects) can be a member of only one OU at a time
- Link GPOs to OUs
- Can delegate control over an OU for an administrator, doesn't matter what OU that admin's user account is in
- Delete an OU, everything inside it is gone
  - Though usually they are delete-protected

## Active Directory Groups
- https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups
- Group type and group scope
- Types: distribution groups and security groups
  - distro group: just an email list with an email list, can't have permissions
  - security groups: permissions and email (mail-enabled security)
- Scope
  - Global 
    - most common way to group users
    - can only contain users from the domain the group was created in
    - but can be given rights to other domains
    - this means that though the group can only have members from it's own domain, it can be used to give those users rights to resources in other domains
  - Domain Local
    - cannot be used to give rights in other domains
    - Add a domain local group on an ACL, then add global groups as members
    - do this to minimize number of entries in an ACL to reduce SID lookups
    - This is called A>G>DL>P
      - Accounts go into global groups, global groups get added to domain-local groups, DLs get assigned permissions on resources
    - sustainable AD design pattern
    - A>G>U>DL>P design
	    - For when you need to add all users for something across the whole forest to a resource
	    - i.e., All Sales Users universal group can then be added to different DL groups across domains
  - Universal
    - replicate forest-wide to all Global Catalog servers
    - when you need a group that has members from across the enterprise in multiple domains
    - add global groups from each domain to a universal group, then add that universal group to all resources where the members of those global groups need access
    - Example: all sales people across the entire company, spanning domains
    - Basically, useful when you need to add different people from across the entire enterprise in a multi-domain environment to the same resource
    - Don't use if you aren't in a multi-domain environment
- Summary:
  - Global groups
    - Members: same domain only
    - Permissions: any domain in the forest
    - Use: organize users by role
  - Domain local groups
    - Members: any domain
    - Permissions: only in the group’s own domain
    - Use: assign access to local resources
  - Universal groups
    - Members: any domain
    - Permissions: any domain
    - Use: add global groups from across domains
    - Replication: forest-wide via the Global Catalog

## Universal Group Membership Caching (UGMC)
- replicate with global catalog servers only
- GC server contains Universal Group List (only on GC)
- Kerberos needs to know which universal groups an account is a member of
  - therefore, authentication needs to check with a GC server
  - all done to build the correct ticket the user will need when authenticated
- problem scenario: authentication is low because of global catalog being in a different site from where the user is authenticating. Therefore need caching of global group membership
- best solution if possible: make the server a GC server
- but if you don't want a lot of additional replication traffic (big environment), there's another solution: enable Universal Group Membership Caching (UGMC)
  - enable on a per-site basis
  - every 8 hours (by default) the DC will sync the Universal Group Membership list (read-only)
  - enable UGMC: AD Sites and Services>Right-click site>NTDS Site Settings>Right-click>Site Settings
    - can also turn global catalog off or on in AD Sites & Services>Site>Servers>right-click server

## Managing users, OUs and groups with PowerShell
- `Get-Command -Noun *search* -Verb *search*`
- `New-ADOrganizationalUnit -Name "Name" -Path distinguishedname`
- `Get-ADUser -Filter *`
- `New-ADUser`
- `New-ADGroup -Name "Name" -GroupCategory category -GroupScope scope -DisplayName "Display name" -Path distinguishedname`

## Group-managed Service Accounts (gMSAs)
- Services have to run under the authority of some account, often the SYSTEM account
- AD manages the password on the account periodically to run a service without an admin doing it manually
- can only use PowerShell
1. Create KDS Root Key (kerberos):

```powershell
Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10)) 

# only do this in a lab. in real life, allow it the 10 hours to replicate across the network:

Add-KdsRootKey -EffectiveImmediately

# It will be immediately available on the DC but will still take up to 10 hours to replicate across the entire network
```

2. After 10 hours, create GMSA:

```powershell
New-ADServiceAccount -Name name -DNSHostName name.yourdomain.com -PrincipalsAllowedToDelegateToAccount group
```

3. On the member server that needs to GMSA: 

```powershell

Add-WindowsFeature rsat-ad-powershell 

Import-Module ActiveDirectory

Install-ADServiceAccount -Identity serviceaccount
```

4. Then go to the service, specify the service account to run it and leave password blank