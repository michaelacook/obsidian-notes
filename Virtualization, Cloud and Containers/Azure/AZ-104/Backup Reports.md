Describing Backup Reports
- insight on backup operations
- can inform forecasting cloud storage consumption
- audit of backup and restore events
- uses Log Analytics Workspace
- config Recovery Services Vault with diagnostics to point to a Log Analytics Workspace

Components
- Log Analytics Workspace 
- Recovery Services Vault with dianostics enabled, sending data to Log Analytics Workspace

Configuring Backup Reports
- Recovery Services Vault>Backup Reports
	- Select/create Log Analytics Workspace
	- Recovery Services Vault>Diagnostic Settings
		- create a Diagnostic setting name
		- select log data to capture
			- for Backup Reports, select:
				- CoreAzureBackup
				- AddonAzureBackupJobs
				- AddonAzureBackupAlerts
				- AddonAzureBackupPolicy
				- AddonAzureBackupStorage
				- AddonAzureBackupProtectedInstance
			- don't use AzureBackupReport
		- Destination details - Send to Log Analytics workspace
			- Destination table
				- Azure diagnostic
				- Resource specific - goes with the log events above
		- Save
		- Recovery Services Vault>Backup Reports>Workspaces
			- Filter reports to get data for Azure Backup Service
			- Can also see summary, backup instances, usage, jobs, etc