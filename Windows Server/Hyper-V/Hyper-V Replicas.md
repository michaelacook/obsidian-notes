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

