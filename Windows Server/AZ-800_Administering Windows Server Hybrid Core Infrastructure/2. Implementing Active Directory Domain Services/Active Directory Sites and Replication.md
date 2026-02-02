#### Active Directory Database Partitions
- The Active Directory database is in a file called `C:\Windows\NTDS\NTDS.dit`
- The database has partitions that replicate differently in the forest and the domain
- Partitions:
	- 1. Configuration
		- Contains info about how the forest is configured. Replicates forest-wide from the forest root domain
	- 2. Schema
		- Contains all object templates and attributes for building objects. Replicates forest-wide from the forest root domain
	- 3. Domain
		- Contains all domain-related object information for specific domains. Replicates to all domain controllers in a domain
		- The Global Catalog is part of this, replicates a subset of all the objects in the domain's Domain partition
	- 4. Application (custom)
		- A custom partition that you create and choose which domain cointrollers get a copy of the information
		- ForestDNSZone
		- DomainDNSZone
#### What is System Volume (SysVol)?

#### What are Active Directory Sites?

#### Active Directory Replication
