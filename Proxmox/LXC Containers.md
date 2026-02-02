# LXC Containers

**Linux Containers (LXC)** provide OS‑level virtualization, allowing you to run lightweight isolated environments without the overhead of a full virtual machine.  Unlike Docker, which focuses on application containers, LXC aims to replicate a complete Linux system environment under a single kernel【793481585297936†L172-L190】.  In Proxmox VE, containers are first‑class citizens alongside virtual machines.

## How LXC works

LXC uses several Linux kernel features to isolate processes:

* **Namespaces** (IPC, UTS, mount, PID, network and user) isolate system resources such as processes, hostnames and filesystems【793481585297936†L194-L203】.
* **cgroups** (control groups) limit CPU, memory and I/O usage, ensuring that no container monopolizes host resources【793481585297936†L225-L237】.
* **AppArmor** or other mandatory access control systems restrict what containers can access at the filesystem and system‑call level【793481585297936†L206-L223】.
* **Chroot/pivot_root** ensures that each container sees its own filesystem hierarchy【793481585297936†L194-L203】.

These features allow containers to behave much like independent systems while sharing the host kernel.  Because containers do not emulate hardware, they have negligible overhead compared to virtual machines.

## When to use containers

Containers are ideal for running Linux services and applications that do not require a custom kernel.  They start quickly, use less memory than VMs and can be consolidated densely.  However, containers cannot run non‑Linux operating systems, and kernel changes on the host affect all containers.  For workloads requiring kernel isolation or custom device passthrough, use a full VM instead; see [[Virtualization Architecture – KVM and QEMU]].

## Creating containers in Proxmox

To create a container via the Proxmox GUI:

1. **Download a template** (e.g., Ubuntu, Debian) via the *Storage → Templates* section.
2. Click **Create CT** and provide a hostname and root password.  Choose the downloaded template as the base image【610956506847632†L190-L256】.
3. Allocate disk space, CPU cores and memory.  Thin‑provisioned storage (LVM‑thin or ZFS) allows snapshots; directories are typically used for ISO images or backups【51867353084062†L40-L63】.
4. Configure networking: assign an IP address, gateway and network bridge such as `vmbr0`.  Proxmox can create VLAN‑aware bridges (see [[Network Bridging and VLANs]])【610956506847632†L190-L256】.
5. Review the summary and click **Finish** to create the container.  You can access the container’s console via the web interface.

The `pct` command provides similar functionality from the CLI (see [[Command‑line Tools]]).  For step‑by‑step instructions see [[Creating Containers]].

### Related notes
- [[Proxmox Overview]]
- [[Virtualization Architecture – KVM and QEMU]]
- [[Creating Containers]]
- [[Network Bridging and VLANs]]
- [[Command‑line Tools]]