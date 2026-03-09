MPIO allows the same iSCSI target to be accessed over redundant paths by the initiator increasing reliability and performance by load balancing data paths across multiple network adapters.

Key concepts:
- **Path** = a unique combination of initiator IP + target IP + target IQN. Two NICs connecting to two target portals = two paths.
- **DSM (Device Specific Module)** — on Windows, MPIO relies on a DSM to define how paths are used. Microsoft provides a generic one (`MSDSM`), which is what you'd use for iSCSI.
- **Load balancing policies** — e.g. _Round Robin_ (distributes I/O across all active paths) vs. _Failover Only_ (one active path, others on standby). This is configurable per disk.
- **Session binding** — in Windows iSCSI, each path corresponds to a separate iSCSI session. This is why you bind specific initiator IPs to specific target portals when setting up MPIO — to ensure each NIC has its own dedicated session/path.

Steps to configure:
1. Create an iSCSI target and virtual disk on your storage server, add at least two network adapters dedicated to storage on the same network that your iSCSI target is on
2. Install the MPIO feature on the initiator with `Install-WindowsFeature -Name Multipath-IO`
3. On the initiator device, open iSCSI Initiator dialog from Server Manager or Windows Tools
4. Allow the iSCSI initiator service to start at boot

![[Pasted image 20260308135052.png]]

4. Open the MPIO properties in Windows Tools and add support for iSCSI devices. You should then see a new device starting with MSFT under MPIO Devices.

![[Pasted image 20260308142745.png]]
![[Pasted image 20260308142846.png]]

5. Add iSCSI target portals under the iSCSI Initiator dialog Discover tab.

![[Pasted image 20260308150019.png]]

6. On the Targets tab, click Connect and check Enable multi-path, then Advanced and select the initiator adapter and IP, and target portal IP. Do this for each SAN adapter you have. In this example both the storage appliance and the initiator have 4 NICs on the SAN subnet, so there will be 4 connections to the target.

![[Pasted image 20260308150626.png]]
![[Pasted image 20260308150744.png]]

You can now see all the connections to your target from the Targets tab. Select Devices to see all the disks and their LUNs. Select MPIO to see the load balancing policy and all the paths for the device. Click on a path ID and select Details to see the source and destination portal IP addresses. For best performance ensure Round Robin is selected as the load balancing policy.

![[Pasted image 20260308151559.png]]