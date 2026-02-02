-  Virtual machine properties:
	- name
	- region
	- size sku
	- image
	- these make up a VM
- then have NIC, disk, temp disk, add additional data disks
- always has a private address, can have a public IP as well

#### Managing Virtual Machine Disks
- VHDs
	- file representation of a real hard disk/SSD
- VHDs utilize the underlying Microsoft storage infrastructure
- they are storaged as Page Blobs in the Blob service
- three purposes for disks:
	1. OS disk - holds operating system
	2. Temporary disk - page/swap, etc
	3. Data disk - persistent application layer data
- two types of disks:
	1. unmanaged
	2. managed
- Major disk types
	- Ultra Disk (SSD)
	- Premium SSD
	- Standard SSD
	- Standard HDD
- disk encryption
	- Storage Service Encryption (SSE)
	- Azure Disk Encryption (ADE)

#### Virtual Machine Availability Sets
- can deploy VMs across availability zones in a scale set for a highly available deployment
- can deploy VMs across fault domains in an availability set to protect against a server rack failure
- update domains - logical groupings of underlying infrastructure to maintenance/updates - can distribute VMs across a max of 3 fault domains
- fault domains - underlying host failure based on power or network outage - can distribute VMs across max 20 update domains

#### Virtual Machine Scale Sets
- auto-scaling to meet traffic demand
- components:
	- VM definition (OS, nics, storage, etc)
	- auto-scaling definition
		- e.g., add another VM when CPU utilization goes above 80% for more than 5 minutes
	- scale-in policy - delete VMs by priority as scaling-in operations occur
- elastic solution

#### Automating VM Deployment
- why automate?
	- automate patching
	- preinstall software at deployment
	- preconfigure settings
- how
	- ARM Templates (IaC)
	- Source VM - golden image
		- prep the VM, install everything, updates, etc
		- generalize (as in using sysprep)
























































































