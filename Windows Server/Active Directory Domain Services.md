[Microsoft Active Directory documentation](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/active-directory-overview)

## Domains, Trees and Forests
- Forest: all domains must be part of a forest and a tree
- A single domain is still within a forest and a tree
- Why have multiple domains? One domain for each country you operate in for instance
- cook.com, then uk.cooklab.com, jp.cooklab.com, etc. They are all part of the same forest and have a trust relationship between them
- Each domain in the forest will have a trust relationship, but will be a separate administrative zone with domain admins who can administer only their own domain
- Enterprise Administrator is a role that can manage all domains in the forest
- AD Tree: all domains share the root domain's name. I.e., cooklab.com, uk.cooklab.com, scotland.uk.cooklab.com
- Multiple trees happen when you have divergent namespaces
- This may happen if your company acquires a totally different company
- Create a trust relationship between the two trees
- i.e., cooklab.com and mikelab.com
- Can't join a domain that was created in one forest to another forest. But if you have a merger or acquisition and you join one domain or tree to another you can still create a trust relationship between them, even if they aren't technically part of the same forest
- Domains part of the same forest share the same schema
- Schema is part of the AD database that defines all the different objects and attributes
- Forest Trust can be set up between different forests so they can share resources, but they won't share the same schema 
- All domains in the same forest will share the Global Catalog, allowing domains to search for objects
- The following objects exist in the forest root domain:
	- The schema master role
	- The domain naming master role
	- The Enterprise Admins group
	- The Schema Admins group
- The following objects exist in each domain (including the forest root):
	- Relative ID (RID) master role
	- Infrastructure master role
	- Primary domain controller (PDC) emulator master role
	- Domain Admins group

## Active Directory Partitions
- https://www.devopsage.com/active-directory-partitions/
- https://itfreetraining.com/handouts/70-640/part2/active_directory_partitions.pdf
- `%SystemRoot%\NTDS.dit` - This is the AD database
- AD is a database which has partitions
- Partitions used to replicate from one DC to another
- Also replicate between domains
- Partitions:
  - **Configuration** partition
    - replicates to every DC in the forest
    - contains info about how the forest is configured
  - **Schema** partition
    - Makes up all object types and attributes for the forest
    - How to build objects, object templates
    - Replicates forest-wide
  - **Domain** partition
    - Unique to the domain
    - Contains all domain-related objects, info
    - Replicates to all DCs in the domain
  - **Application** partition
    - Custom created by the admin, choose which DCs it replicates to
    - `ForestDNSZone`, `DomainDNSZone` partitions come with AD these days
- Global Catalog - special role assigned to a DC, replicates a subset of all objects in every domain's "Domain" partition
  - Locate objects in different domains, such as user accounts
  - Doesn't replicate every attribute of those objects, but enough to find objects across domains

## Read-only Domain Controllers
- Why use a RODC?
  - One-way replication for sites that don't have on-site IT staff
  - Prevents corrupted changes from replicating domain-wide
  - Doesn't cache admin accounts, does authentication pass-back to a DC in another site
  - Cache passwords just for certain users
  - Can be used for local DNS in the site so that DNS doesn't have to go across the WAN
  - Isn't always used
- Pre-staging
  - Tools>AD Users and Computers>Domain Controllers
    - Right-click>Pre-create RODC account...
    - Go through prompts, specify a user who can set up the server
    - All that user needs to do is name the server with the correct name you specify
- Password replication for RODC - right-click the RODC computer in AD and select Password Replication Policy

## Using PowerShell for remote administration
- WinRM (Windows Remote Management) service must be running
- TCP 5985 HTTP (unencrypted)
- TCP 5986 HTTPS (encrypted)
- Get-Service -Name winrm
- winrm quickconfig to check if it's running on the remote host
- Possible to turn on winrm by group policy
- Use `-ComputerName` option to specify the remote host when running a cmdlet
  - i.e., Get-Service `-ComputerName` name
- Invoke-Command `-ComputerName name -ScriptBlock {}` to run a script against a remote machine
  - Can also specify a script location on the network
- `Enter-PSSession -ComputerName` name to enter a remote PowerShell session
- Can also run commands against multiple remote hosts at a time with a comma-separated list computer names

## Flexible Single Master Operations roles (FSMO)
- https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/fsmo-roles
- https://www.youtube.com/watch?v=tUoSwjTwXpc
- Five roles for jobs that cannot be duplicated
- All five roles will start on the first domain controller for the forest
- They can all be on one server, or spread out
- Two are forest-level, other are domain-level
- Forest level:
  - Domain Naming Master
    - Handles configuration partition of AD
  - Schema Master
    - Master copy of the schema database
- Domain level: 
  - RID Master
     - Relative ID Master
     - SID and RID - security ID and unique ID
     - in charge of giving out RIDs to objects
     - RID pools get issued to DCs by the RID Master
     - Ensure each object has a unique RID
  - Infrastructure Master
     - Handles group-to-user references
     - Linking groups
     - Should not be run on a Global Catalog server unless every DC is a global catalog
  - PDC Emulator Master
     - Handles password changes
     - DCs check with PDC Emulator Master for passwords
     - Acts as a PDC for old NT Servers 
     - **Handles time synchronization needed for Kerberos**
     - Handles GPOs, master writable copies
- Every domain gets these roles, they aren't forest-wide
- Every domain controller has a read-only copy of all these roles, but each role is active on only one DC at a time
- Can transfer the roles from one server to another
- If a DC hosting a master role goes down, another server can seize the role by activating the read-only copy
- Can see RID Master, PDC Emulator Master and Infrastructure Master in AD Users and Computers by right-clicking domain and selecting Operations Masters...
- Can see the Domain Naming Master in AD Domains and Trusts by right-clicking at the top of the left pane and select Operations Master...
- Microsoft has hidden the schema tool, so you need to register a DLL
  - Run regsvr32 schmmgmt.dll in Run dialog
  - Then open mmc.exe and add Active Directory Schema
  - Right-click Active Directory Schema in left pane of the MMC and select Operations Master...
- To transfer a FSMO role, you need to be on the server you wish to transfer the role to
- To seize a role, open command prompt
  - ntdsutil and hit enter to see available commands
  - roles
- Can also use a PowerShell command
  - `Move-ADDirectoryServerOperationsMasterRole -identity server -OperationsMasterRole role`
  - `- Force` option is to seize the role; otherwise transfers the role
- See which server hosts which FSMO role with `netdom query fsmo`









