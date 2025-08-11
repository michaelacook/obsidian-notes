#### What is a Forest?
- foundational, every domain is part of a forest
- each domain has at least one domain controller
- each domain has a DNS name
- a domain can have a child domain, which is a subdomain of it's parent
- domains that share a root DNS name are called a domain tree
- first root domain in a forest is called the Forest Root Domain
- additional root domains are called Tree Root Domains and have a different DNS name

![[Pasted image 20240802161822.png]]

- key facts about AD DS Forests:
	- Security boundary. All domains in a forest trust each other. external networks are untrusted
	- Two privileged groups exist at the root of the Forest root domain:
		- Enterprise Admins group - most privileged
		- Schema Admins - responsibile for maintaining the schema
	- Replication boundary. Forest shares a schema and global catalog; schema is the definition of the objects and properties that can be assigned to an object; global catalog is a reference to every object in the Forest - like an address book
#### Deploy AD DS Forests
- Need to be a member of local Administrators group on the server that will be the first DC in the forest in order to deploy AD DS, and the built-in administrator will be added to Enterprise Admins and Schema Admins groups
- Need to provide a DNS name and NetBIOS name (limited to 15 chars). DNS name should be a subdomain of the organization's external DNS name 
- Need to provide a **Directory Services Restore Mode (DSRM) password** - this is highly sensitive, used to recover AD database from a backup
- First domain controller needs to be a DNS server, and it's a best-practice for all DCs to also be DNS servers

#### Deploy AD DS with PowerShell

```ps
%% PowerShell commands to configure a new forest %%

Install-WindowsFeature AD-Domain-Services

Import-Module ADDSDeployment

Install-ADDSForest
```
