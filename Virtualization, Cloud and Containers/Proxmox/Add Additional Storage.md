You would rarely if ever want to store VMs, VM backups or ISOs on the boot device for Proxmox. This short article details how to add additional storage.

### How To

1. Wipe and initialize disk - this is necessary
	1. Use fdisk in Proxmox in a console session
	2. Can also use the graphical tool in Disks
2. Add disk as a Directory in Disks>Directory

A good YouTube video explaining the process can be found [here](https://www.youtube.com/watch?v=HqOGeqT-SCA).

