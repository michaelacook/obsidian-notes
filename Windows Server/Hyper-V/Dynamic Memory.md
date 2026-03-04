
## The Core Idea

Think of Dynamic Memory like a **shared pool of RAM** on the host. Instead of permanently locking RAM to each VM, Hyper-V lends RAM out as needed and takes it back when it's not being used.

> **Analogy:** It's like a team sharing a set of company laptops. Not everyone needs one at the same time, so they're shared based on who needs one right now — rather than giving everyone a permanent laptop that sits idle most of the day.

---

## The Three Settings You Need to Know

### 1. Startup RAM

- The amount of RAM the VM gets **when it first boots**
- Once booted, Hyper-V will adjust from here based on demand
- Set this high enough that the VM can actually boot without struggling

### 2. Minimum RAM

- The **lowest** amount of RAM Hyper-V is allowed to give the VM
- Even if the VM is completely idle, it will never drop below this
- Don't set this too low — Windows needs a baseline to stay stable

### 3. Maximum RAM

- The **ceiling** — Hyper-V will never give the VM more than this
- The VM can request more RAM, but it won't get it if it's already at the maximum
- This is your safety net to stop one VM consuming all host RAM

---

## How It Actually Works Day-to-Day

```
VM boots          → Gets Startup RAM
VM sits idle      → Hyper-V slowly reclaims unused RAM (down to Minimum)
VM gets busy      → Hyper-V allocates more RAM (up to Maximum)
Host runs low     → Hyper-V aggressively reclaims RAM from idle VMs
```

The VM's OS doesn't really "know" this is happening — Hyper-V handles it transparently via the **Memory Balloon Driver** (installed with Integration Services).

---

## Memory Pressure — What Is It?

Hyper-V constantly measures **memory pressure** on each VM. This is simply a measure of how hard the VM is working to find free RAM.

|Pressure Level|What It Means|Hyper-V Response|
|---|---|---|
|Low|VM has plenty of free RAM|Reclaim some back to the host|
|Optimal|VM is comfortable|Leave it alone|
|High|VM is struggling for RAM|Allocate more RAM to the VM|

---

## The Memory Buffer Setting

This is an often-overlooked setting. It tells Hyper-V to keep a small **reserve of extra RAM** available for the VM above what it's currently using.

- Default is **20%** — meaning if the VM is using 1GB, Hyper-V tries to keep 1.2GB assigned
- This prevents the VM from having to wait for RAM to be allocated the moment it needs it
- Think of it as keeping a spare in the boot — you hope you don't need it immediately, but it's there

---

## Smart Ballooning

The **Memory Balloon Driver** (part of Hyper-V Integration Services) is what actually moves RAM between the host and VM.

- When Hyper-V wants RAM **back** from a VM → it **inflates** the balloon inside the VM (taking up space, forcing Windows to free other RAM)
- When Hyper-V wants to **give** RAM to a VM → it **deflates** the balloon (freeing up space inside the VM)

> The balloon is invisible to the VM's applications — they just see available and unavailable RAM as normal.

---

## Common Mistakes to Avoid

**Setting Minimum RAM too low** The VM will become unstable or unresponsive if it drops below what Windows needs to operate. A safe minimum for a Server 2022/2025 VM is around 512MB–1GB depending on role.

**Setting Maximum RAM too high across many VMs** If every VM's maximum adds up to more RAM than the host has, you're over-committing. This is fine in small amounts but can cause host instability if many VMs spike simultaneously.

**Not installing Integration Services** Without the Memory Balloon Driver, Dynamic Memory cannot function. Always ensure Integration Services are installed and up to date.

**Using Dynamic Memory with certain roles** Some roles don't play well with Dynamic Memory, including:

- Domain Controllers (can cause replication issues)
- VMs running SQL Server (SQL pre-allocates RAM aggressively)
- VMs with RemoteFX (GPU virtualisation) For these, use **Static Memory** instead.

---

## Quick Reference

|Setting|Plain English|
|---|---|
|Startup RAM|RAM at boot|
|Minimum RAM|Never go below this|
|Maximum RAM|Never go above this|
|Memory Buffer|Keep a small spare on top|
|Memory Pressure|How desperate is the VM for RAM?|
|Balloon Driver|The mechanism that moves RAM in/out|