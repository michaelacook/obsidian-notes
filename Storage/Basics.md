## The Big Picture: What “Storage” Really Is

At its core, storage answers **three questions**:

1. **How is data accessed?** (block vs file vs object)
2. **How is data protected?** (RAID, replication, snapshots)
3. **How is performance managed?** (IOPS, latency, throughput)

Think of storage like networking:

* **Block storage** ≈ raw Ethernet frames
* **File storage** ≈ TCP + application protocol
* **Object storage** ≈ REST APIs

---
## Storage Access Models (VERY important)

## 2.1 Block Storage

**Definition:** Raw blocks of data presented to a host as a disk.

**Examples**

* SAN
* iSCSI
* Fibre Channel
* NVMe-oF

**Key traits**

* OS formats it (NTFS, ext4, XFS)
* Highest performance
* Required for databases, VM disks

**Networking analogy**

> Block storage is like giving the host a NIC and saying “figure out the packets yourself.”

## 2.2 File Storage

**Definition:** Storage that understands files and directories.

**Examples**

* SMB / CIFS (Windows)
* NFS (Linux/Unix)

**Key traits**

* Shared access
* Permissions handled by the storage
* Easier to manage, slightly slower

**Networking analogy**

> File storage is like a load balancer that already understands HTTP.

## 2.3 Object Storage

**Definition:** Data stored as objects with metadata, accessed via API.

**Examples**

* S3
* Azure Blob
* Ceph Object

**Key traits**

* Massive scale
* No traditional filesystem
* Not suitable for databases or VMs

**Networking analogy**

> Object storage is like a REST service, not a filesystem.

---
## Disk Types & Media

## 3.1 HDD (Hard Disk Drives)

* Cheap
* High capacity
* High latency
* Terrible random I/O

**Used for:** Archives, backups

## 3.2 SSD

* Flash-based
* Low latency
* Limited write endurance

### SATA SSD

* Bottlenecked at ~550 MB/s

### NVMe SSD

* Uses PCIe
* Massive IOPS
* Extremely low latency

## 3.3 Endurance (DWPD)

**DWPD (Drive Writes Per Day)**

* How many full writes per day the disk can handle
* Important for databases and virtualization

---

## Performance Metrics (Storage ≠ Bandwidth)

## 4.1 IOPS

**Input/Output Operations Per Second**

* Small random reads/writes
* Critical for databases and VMs

> 10 GbE doesn’t matter if your disks can only do 5k IOPS.

## 4.2 Latency

**Time to complete an I/O**

* Measured in microseconds or milliseconds
* Most important metric

**Rule of thumb**

* HDD: ~5–10 ms
* SATA SSD: ~1 ms
* NVMe: <100 µs

## 4.3 Throughput

**MB/s or GB/s**

* Large sequential transfers
* Important for backups and media workloads

## 4.4 Read vs Write

Writes are:

* Slower
* More complex (parity, mirroring)
* More destructive to SSDs

---

## RAID & Data Protection

## 5.1 RAID (Redundant Array of Independent Disks)

| RAID | Description     | Pros             | Cons          |
| ---- | --------------- | ---------------- | ------------- |
| 0    | Striping        | Fast             | No protection |
| 1    | Mirroring       | Simple           | 50% capacity  |
| 5    | Parity          | Efficient        | Slow writes   |
| 6    | Dual parity     | Safer            | Even slower   |
| 10   | Stripe + mirror | Best performance | Expensive     |

**Modern truth**

> RAID is about **availability**, not backup.

## 5.2 Rebuilds

* Rebuilding large disks can take **days**
* During rebuild, performance tanks
* Another disk failure = data loss (depending on RAID)

---

## Logical Storage Abstractions

## 6.1 Disk Pool

Collection of physical disks treated as one resource.

## 6.2 LUN

**Logical Unit Number**

* A block device presented to a host
* Like a “virtual disk”

## 6.3 Volume

* Usable storage carved from a pool
* May contain a filesystem

---

## Thin vs Thick Provisioning

## Thin Provisioning

* Space allocated on demand
* Risk of overcommit

## Thick Provisioning

* Space reserved upfront
* Predictable performance

**Networking analogy**

> Thin provisioning is oversubscribing bandwidth.

---

## Snapshots & Clones

## Snapshot

* Point-in-time reference
* Usually copy-on-write
* Very fast

⚠ Not a backup by itself.

## Clone

* Writable copy of a snapshot
* Used for test/dev, VDI

---

## Replication & DR

## 9.1 Synchronous Replication

* Writes must commit on both sides
* Zero data loss
* High latency sensitivity

## 9.2 Asynchronous Replication

* Lag allowed
* Better performance
* Some data loss possible

**Key terms**

* **RPO** – how much data you can lose
* **RTO** – how fast you can recover

---

## Multipathing (MPIO)

**Definition:** Multiple physical paths to the same storage.

* Active/Active
* Active/Passive

**Why it matters**

* Redundancy
* Load balancing

**Networking analogy**

> ECMP for storage traffic.

---

## Filesystems (high level)

* NTFS, ReFS (Windows)
* ext4, XFS, ZFS (Linux)
* ZFS combines filesystem + volume manager

**Key concepts**

* Journaling
* Checksums
* Copy-on-write (ZFS, ReFS)

---

## Caching & Tiers

## Cache

* RAM or SSD accelerating reads/writes

## Tiering

* Hot data → fast disks
* Cold data → slow disks

Automatic on modern arrays.

---

## Protocols & Transports

| Protocol      | Type                |
| ------------- | ------------------- |
| iSCSI         | Block over TCP      |
| Fibre Channel | Block over FC       |
| NVMe-oF       | Block over RDMA/TCP |
| SMB           | File                |
| NFS           | File                |
| S3 API        | Object              |

---

## Storage Is About Failure

Storage design assumes:

* Disks will fail
* Controllers will fail
* Links will fail
* Humans will make mistakes

Good storage = **controlled failure**.

---

## Mental Model (Most Important)

If you remember nothing else:

> **Latency beats bandwidth**
> **IOPS beats throughput**
> **RAID is not backup**
> **Snapshots are not backups**
> **Disks always fail**

