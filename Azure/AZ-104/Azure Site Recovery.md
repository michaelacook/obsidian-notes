acDescribing Azure Site Recovery
- [[Disaster Recovery]] solution
- automate recovery from a primary to secondary location
- needs Azure Recovery Services Vault
- cross-zone and cross-region recovery
- can also be used with on-prem resources

Components
- VMs in one region/AZ - needs to failover to a target rg in the same geography
- cache data gets stored and replicated to target location, allowing for failover

Configuring Site-to-Site Recovery
- Recovery Services Vault>Site Recovery>Enable replication
	- configure source location, sub, resource group
	- select source VMs
	- configure replication settings for target location
- go to Vault and Replicated items to test failover, failover, change recovery point, disable replication for specific VMs, etc
	- per VM instance
- can create a Recovery Plan for failing over many VMs together - create in the vault

Key Takeaways
- Replicated Items 
	- workload that will be replicated site-to-site by Azure Site Recovery
- Replication Policy
	- defines the frequency of snapshots and retention period of recovery points
	- can be app-consistent or crash-consistent
- Recovery Plan
	- automate and run test failover events with multiple protected items and pre- and/or post-scripts
	- for failing over many VMs together