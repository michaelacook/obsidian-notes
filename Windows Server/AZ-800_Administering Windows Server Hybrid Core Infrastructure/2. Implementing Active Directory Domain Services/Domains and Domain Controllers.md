#### What is an AD DS Domain?
- Replication boundary. changes made to objects in a domain are replicated within the domain
	- some objects are replicated outside the domain to the rest of the forest: global catalog and schema
- An administrative unit. Contains privileged users and groups specific to the domain, don't have ability to manage other domains
	- Domain Admins group - manage all objects in the domain and AD database
	- Local administrator, in the Domain Admins group
		- Local administrator in root domain is also a member of the Enterprise Admins and Schema Admins groups
	- Exam tip: must be a member of Enterprise Admins group to add a new domain to an existing forest

#### What is a Domain Controller?
- Provides authentication and authorization for domain resources
- Store and replicate AD database that contains users, groups, computers, printers and other objects
- Each DC stores a copy of the database and can make changes to it
- Exam tip: must be a member of the Domain Admins group to deploy a new domain controller

#### Deploying a child domain

#### Performing a Domain Join
- Two methods:
	- Online - computer has network access to a domain controller
		- Requires network connectivity and DNS configuration
		- Any Active Directory user can join a computer to the domain up to 10 times
		- Domain Admins and Administrators do not have the join limit.
	- Offline - no network connectivity required
		- Perform join without contacting a domain controller
		- Useful in disconnected environments or when launch speed is important
		- Doesn't require a reboot
		- Steps:
			- 1. Provision a computer account, save metadata to a text file
			- 2. Use text file to complete the join
- Exam tip: command prompt tool `Djoin` is used to perform an offline join
	- Steps:
		- 1. Open CMD
		- 2. Run `djoin /provision /domain domainname.com /machine computername /savefile C:\filename.txt `
		- 3. Copy the metadata file to the offline client computer
		- 4. On the domain controller, run `djoin /requestODJ /loadfile C:\filename.txt /windowspath %systemroot% /localos`
- Can perform a domain join with PowerShell with `Add-Computer -DomainName domainname.com`
- Note: make sure there is a conditional forwarder to the parent domain set on the child domain server

#### Steps to deploy a new domain controller
- Microsoft likes to test the steps and order of the steps
- 1. Login as a Domain Admin
- 2. Perform initial server configuration
	- Set a static IP, DNS, configure computer name, updates, etc
- 3. Join the server to the existing domain
- 4. Install the Active Directory Domain Services role
- 5. Promote the server to a domain controller
- 6. Restart new domain controller
- If you want to install AD DS from installation media rather than replicating AD over the network, you have to create installation media on the primary DC by logging on interactively and using the `ntds` commandline tool
