Replication is a disaster recovery feature for virtual machines. The replica is an exact copy of the source VM that can failover on another Hyper-V host in the event of a catastrophic failure. Failover assumes that the source VM is lost and unrecoverable, and the replica becomes the new "official" copy.

### Configuring Replication on the Host
- Enable Replica server on the host that will store replicas
- Kerberos is okay for a lab, in production try to set up certificate services and use HTTPS

![[Pasted image 20260310223637.png]]

- Specify the server from which VMs can replicate as well as a replication group name

![[Pasted image 20260310223829.png]]

- Ensure TCP port 80 is open 

![[Pasted image 20260310223917.png]]![[Pasted image 20260310224159.png]]

### Configuring Replication for a VM

- Enable replication for the VM and proceed through the wizard to configure as per your organization's needs.

![[Pasted image 20260310225829.png]]

### Failover

#### Planned Failover
- If you need to failover VMs to another host because you need to take the host offline for maintenance, right-click the VM in a shutdown state and select Replication>Planned Failover...
	- Ensure you reverse the direction of replication, otherwise changes to the VM after failover will not be preserved when you failback to the primary host
	- Once finished with maintenance, find the VM on the secondary host, and go through the exact same process again

#### Unplanned Failover
- Do not do an unplanned failover unless it's absolutely necessary, as in cases where there is no chance the primary VM host can be recovered, or also a power outage or other scenario where there is no choice but to initiate failover from the replica
- On the replica server, right-click the replica and select Replication>Failover...
- Select a recovery point. You may lost a bit of data because your replica will be good up to the last 5 to 15 minutes before host failure
- Select Replication>Cancel Failover in cases where the replica has issues and can't be used and you need to failover again with a different recovery point
- If the failed host is able to be recovered at a later point, you can select Replication>Reverse Replication... and then perform a planned failover back to the original host
- Remove recovery points if they are corrupted or you just don't need them
- If the failed host is never going to come back up, you can remove the replication relationship by right-clicking Replication>Remove Replication
	- If you recovered the failed host and wanted to recreate the replica from scratch

