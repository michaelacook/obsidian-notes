# Virtualization Architecture – KVM and QEMU

Proxmox VE uses a hybrid virtualization architecture that combines **KVM** and **QEMU**.  Understanding the roles of each component helps explain why Proxmox achieves near‑native performance while still supporting a wide range of guest operating systems.

## KVM: hardware‑accelerated virtualization

KVM (Kernel‑based Virtual Machine) is a Linux kernel module that turns the Linux kernel into a type‑1 hypervisor.  KVM provides hardware‑assisted virtualization by exposing virtualization extensions of the host CPU (such as Intel VT‑x or AMD‑V) to guest operating systems.  In Proxmox, KVM handles CPU and memory virtualization: guest code runs directly on the physical CPU, and memory is allocated with support for features such as huge pages and NUMA awareness【793481585297936†L128-L168】.  Because KVM runs in the kernel, it delivers low overhead and near‑native performance.

## QEMU: device emulation and I/O

QEMU (Quick EMUlator) is a user‑space program that emulates computer hardware devices.  It provides virtual devices—such as storage controllers, network interfaces, BIOS, and video adapters—that guest operating systems interact with.  When combined with KVM, QEMU passes CPU instructions to the host CPU while retaining responsibility for emulating I/O devices.  Administrators can pass an ISO image to QEMU and the guest OS perceives it as a real CD‑ROM【793481585297936†L145-L154】.  By delegating device emulation to QEMU and CPU virtualization to KVM, Proxmox achieves both flexibility and performance【793481585297936†L156-L168】.

## Hybrid operation

In Proxmox VE, KVM and QEMU are tightly integrated.  When a virtual machine is started, KVM handles the virtualization of CPU and memory instructions, while QEMU emulates the rest of the hardware.  This configuration means that Proxmox behaves similarly to a type‑1 hypervisor even though QEMU runs in userspace【793481585297936†L162-L168】.  The hybrid model also allows advanced features such as live migration, snapshots and device passthrough because QEMU’s userspace nature provides flexibility for device management.

Proxmox’s virtualization stack is fully open source and supports a wide range of guest operating systems, including Windows, Linux and BSD.  Administrators can enable nested virtualization, CPU pinning, huge pages and other performance optimizations via the **qm** command‑line tool or the web interface.  For container‑based virtualization see [[LXC Containers]].

### Related notes
- [[Proxmox Overview]]
- [[Creating Virtual Machines]]
- [[Snapshots, Cloning, and Migration]]
- [[Command‑line Tools]]
- [[LXC Containers]]