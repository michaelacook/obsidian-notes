Describing Azure Backup
- Backup as a Service 
	- in the cloud
	- on-premises
		- Hyper-V workloads
		- VMWare workloads
- Need an Azure Recovery Services Vault - create backup policy
	- then backup data
	- restore from backup
- Supported workloads
	- Azure VMs
	- On-prem VMs
	- SQL Server
	- SAP HANA

Components of Azure Backup
- Compute workload - either on prem or in the cloud
- Recovery Services Vault
	- create Azure Backup Policy
	- run scheduled backups based on policy

Demonstration
- Go to VM, left pane Operations>Backup
- Create Recovery Services Vault and backup policy
- do this from within the VM view in the portal
- VM snapshots (restore points)
	- Crash Consistent
	- Application Consistent
	- File-system Consistent
- Restoring a VM
	- VMs need to be in a stopped state when restoring a backup
	- Need storage for a staging location
		- Create if needed
		- needs to be in the same region as the Recovery Services Vault
	- Operations>Backup>Restore VM
		- Select restore point
	- Restore Configuration
		- Replace existing or create new from the backup
		- Select options, staging location
- Recovery Services Vault>Backup Jobs - see backups and restores currently running and completed