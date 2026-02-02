# Proxmox Overview

**Proxmox Virtual Environment (VE)** is an open‑source virtualization platform built on a Debian Linux distribution and an optimized kernel.  The system allows administrators to run both full virtual machines and lightweight containers on the same host.  Under the hood, Proxmox uses **KVM** (Kernel‑based Virtual Machine) and **QEMU** in a hybrid architecture: KVM provides hardware‑assisted CPU and memory virtualization, while QEMU emulates devices and manages I/O【793481585297936†L128-L168】.  This combination delivers near‑native performance for VMs because CPU operations execute directly on the host CPU and only device emulation is handled in userspace【793481585297936†L128-L168】.

Proxmox VE incorporates **LXC containers** so that administrators can run Linux workloads without the overhead of a full virtual machine.  LXC leverages kernel features such as namespaces and cgroups to isolate processes, offering OS‑level virtualization with minimal overhead【793481585297936†L172-L203】.  For more about containerization see [[LXC Containers]].

From an architectural perspective, Proxmox VE integrates compute, storage and networking resources into a unified management stack.  It supports many storage technologies—local directories, logical volume manager (LVM), thin provisioning, ZFS and Ceph—allowing administrators to tailor performance and redundancy to their workloads【51867353084062†L40-L63】.  Network management is handled through software‑defined bridges, VLANs and firewall rules, enabling flexible network topologies and isolation; this is covered in [[Network Bridging and VLANs]].

Proxmox provides a **web‑based GUI** that uses AJAX for real‑time status updates and supports SSL‑encrypted connections.  Administrators can manage hundreds of VMs or containers, monitor nodes in a cluster, assign role‑based permissions and access VMs via an HTML5 console【793481585297936†L328-L367】.  The platform also exposes a REST API and command‑line tools for automation; see [[Command‑line Tools]] for details.

**Clustering** is a core part of the platform.  Multiple nodes can be joined into a single **multi‑master cluster** where configuration data is replicated in real time using the Proxmox cluster file system (pmxcfs).  Clusters simplify management by providing a central view of all nodes and enabling live migration, high‑availability services and cluster‑wide firewall rules【186625474225790†L327-L356】.  More about clustering can be found in [[Clustering and pmxcfs]].

Proxmox VE is free and open source.  The enterprise edition offers support subscriptions but the core functionality is available without license fees【762502861043796†L216-L256】.  Its flexible architecture, community ecosystem and support for diverse workloads make Proxmox a compelling alternative to proprietary virtualization platforms.

### Related notes
- [[Virtualization Architecture – KVM and QEMU]] – deeper explanation of how KVM and QEMU work together
- [[LXC Containers]] – explains OS‑level containerization and how to create containers
- [[Storage Types]] – discusses local storage, LVM, ZFS and other storage backends
- [[Ceph Integration]] – covers Ceph as a hyper‑converged storage backend
- [[Clustering and pmxcfs]] – explains clusters, pmxcfs and high‑availability
- [[Creating Virtual Machines]] – step‑by‑step guide for creating VMs
- [[Creating Containers]] – step‑by‑step guide for LXC containers
- [[Backup and Restore]] – describes backup strategies and restore procedures
- [[Snapshots, Cloning, and Migration]] – covers snapshotting, cloning, migration and resizing
- [[Network Bridging and VLANs]] – network configuration and VLANs
- [[User and Permission Management]] – how to add users and assign roles
- [[Command‑line Tools]] – overview of the main CLI tools
- [[Graphical User Interface]] – summary of Proxmox VE’s GUI features