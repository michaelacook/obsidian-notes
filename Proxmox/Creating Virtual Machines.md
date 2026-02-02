# Creating Virtual Machines

This note describes how to create a virtual machine (VM) in Proxmox using the web interface.  VMs run complete operating systems and are backed by disk images stored on local storage, LVM‑thin, ZFS or Ceph (see [[Storage Types]]).

## Upload an ISO image

Before creating a VM you need an installation image.  To upload an ISO file:

1. Log into the Proxmox GUI at `https://<host>:8006` as root.
2. In the node tree, select **local (node) → ISO Images**.
3. Click **Upload** and browse to the ISO file on your workstation.  Once uploaded, the ISO appears in the `ISO Images` list and the task viewer shows a “TASK OK” message【461190505236629†L397-L435】.

## Create the VM

1. Click **Create VM** in the top right corner of the Proxmox interface【461190505236629†L445-L449】.
2. **Name** – provide a meaningful name for the VM and click **Next**【461190505236629†L452-L456】.
3. **OS** – choose the uploaded ISO under the **OS** tab; this sets the boot media【461190505236629†L459-L463】.
4. **System** – configure firmware (BIOS/UEFI), enable the QEMU guest agent if desired and leave other settings at their defaults【461190505236629†L468-L475】.
5. **Disks** – select the storage pool (Directory, LVM‑thin, ZFS or Ceph) and specify the disk size.  Thin‑provisioned pools allow over‑allocation【461190505236629†L477-L485】.
6. **CPU** – allocate the number of cores and choose a CPU type; selecting **host** passes through the host CPU features【461190505236629†L487-L494】.
7. **Memory** – specify RAM in MiB or GiB【461190505236629†L496-L503】.
8. **Network** – select a bridge (e.g., `vmbr0`), optionally enable VLAN tagging and configure the model (virtio for best performance)【461190505236629†L506-L509】.  For information on bridges see [[Network Bridging and VLANs]].
9. **Confirm** – review the summary and click **Finish**【461190505236629†L513-L516】.

Proxmox creates the VM with the chosen hardware profile.  To start it, select the VM and click **Start**, then open the **Console** to install the operating system【461190505236629†L529-L536】.  Once the OS is installed, install the QEMU guest agent for improved performance and status reporting.

## Tips

* Use **virtio** drivers for disk and network devices to maximize performance.
* Allocate resources conservatively; you can adjust CPU, memory and disk later via the **Hardware** tab or using the `qm` CLI.
* Use thin‑provisioned storage (LVM‑thin or ZFS) to save space and enable snapshots (see [[Snapshots, Cloning, and Migration]]).
* When using Ceph, choose the Ceph storage pool for VM disks to enable live migration across nodes【162636434422459†L134-L139】.

### CLI alternative

Advanced users can create and manage VMs via the command line using the `qm` tool.  For example:

```bash
qm create 100 --name myvm --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0 --ide2 local:iso/ubuntu-24.04.iso,media=cdrom --scsihw virtio-scsi-pci --scsi0 local-lvm:8
qm start 100
```

See [[Command‑line Tools]] and [[Snapshots, Cloning, and Migration]] for more commands.

### Related notes
- [[Proxmox Overview]]
- [[Virtualization Architecture – KVM and QEMU]]
- [[Storage Types]]
- [[Network Bridging and VLANs]]
- [[Snapshots, Cloning, and Migration]]
- [[Backup and Restore]]