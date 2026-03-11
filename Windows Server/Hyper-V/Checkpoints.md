# Hyper-V Checkpoints – Concise Reference Guide

---

## Checkpoint Types

|Type|How It Works|Use Case|
|---|---|---|
|**Standard**|VSS-unaware crash-consistent snapshot. Captures RAM + disk state at a point in time.|Dev/test, non-production VMs|
|**Production**|Uses VSS (Windows) or FSFREEZE (Linux) inside the guest for application-consistent state. No RAM state saved by default.|Production VMs, SQL/Exchange|

> **Set in:** VM Settings → Management → Checkpoints. Production is the default since Server 2016.  
> Production checkpoints have an option to **fall back to standard** if VSS fails — disable this on production systems if consistency is non-negotiable.

---

## Checkpoint File Types

|File|Extension|Purpose|Default Location|
|---|---|---|---|
|Checkpoint configuration|`.vmcx`|VM config state at checkpoint time|`Snapshots\` subfolder of VM config path|
|Runtime state|`.vmrs`|Saved memory/CPU state (standard checkpoints only)|`Snapshots\` subfolder|
|Differencing disk|`.avhdx`|Captures all disk writes after the checkpoint|Same folder as the parent `.vhdx`|
|Checkpoint metadata|`.xml`|Legacy format (pre-2016) config data|`Snapshots\` subfolder|

> ⚠️ The `.avhdx` lives **alongside your base disk**, not in the Snapshots folder. When you have multiple checkpoints, Hyper-V builds a chain: `base.vhdx → c1.avhdx → c2.avhdx`.

---

## Creating Checkpoints

1. In Hyper-V Manager, right-click the VM → **Checkpoint** (or use the action pane).
2. The checkpoint appears in the **Checkpoints** pane on the VM's detail page.
3. Rename it immediately — double-click the checkpoint name. Default names are timestamps and get confusing fast.

> The VM keeps running. A new `.avhdx` is created and all subsequent writes go there.

---

## Merging (Deleting) Checkpoints

Hyper-V doesn't use the word "merge" in the UI — **deleting a checkpoint triggers the merge**.

### To apply/keep current state and remove a checkpoint:

- Right-click checkpoint → **Delete Checkpoint** — merges that `.avhdx` into its child or parent disk. VM stays running; merge happens in the background.
- Right-click → **Delete Checkpoint Subtree** — removes that checkpoint and all children beneath it.
	- This is the way to merge checkpoints
	- To merge all checkpoints, right-click the base of the checkpoint subtree and click Delete Checkpoint Subtree

### To revert to a checkpoint:

- Right-click → **Apply** — rolls the VM back. You'll be prompted to create a checkpoint of the _current_ state first (do this if needed).

### Merge timing:

- Merges happen **while the VM is running** for most cases.
- If the VM is off, the merge completes synchronously before the VM can start.
- Large `.avhdx` files on spinning disk can cause a noticeable merge delay. Monitor disk I/O.

> ⚠️ **Never manually delete `.avhdx` files** from Explorer/disk. Always delete through Hyper-V Manager to allow proper chain cleanup.

---

## Gotchas & Edge Cases

### Moving Storage (Storage Migration / Export)

- If you **move VM storage** while checkpoints exist, Hyper-V migrates the entire checkpoint chain including all `.avhdx` files.
- The move will **fail or warn** if the chain is broken or files are missing.
- Best practice: **delete all checkpoints before migrating storage** to avoid chain complexity and reduce transfer size.

### Live Migration

- Live migration supports checkpoints — the chain travels with the VM.
- However, a long checkpoint chain can significantly increase migration time.

### Hyper-V Replica

- Checkpoints on a replica VM are **recovery points**, not standard checkpoints — managed separately under Replication settings.
- During a **planned failover**, checkpoints are generally handled cleanly.
- During an **unplanned failover**, the replica VM may be in a crash-consistent state; recovery point checkpoints allow you to pick a restore point on the replica side.
- After failover, clean up old recovery checkpoints before re-enabling replication.

### VHD Sets (`.vhds`) — Shared VHDX for Guest Clusters

- Checkpoints of VMs using shared VHD Sets behave differently — the shared disk is **excluded from the checkpoint** by design (it's shared across nodes).
- Only the non-shared disks are captured.

### Integration Services

- Production checkpoints require **up-to-date Integration Services** in the guest. Outdated IS = failed VSS = possible silent fallback to standard (if fallback is enabled).

---

## Best Practices

- **Production VMs:** Always use Production checkpoints. Disable the standard fallback option.
- **Keep chains short:** No more than 3–5 deep. Long chains hurt disk I/O and complicate recovery.
- **Never use checkpoints as backups.** Use Windows Server Backup, Azure Backup, or Veeam. Checkpoints are not offsite, not independent, and chained to a live disk.
- **Delete before storage moves.** Always merge checkpoints before moving or exporting VM storage.
- **Rename on creation.** Timestamps alone are useless after the third checkpoint.
- **Monitor disk space.** `.avhdx` files grow unbounded until merged. A busy VM can fill a volume fast.
- **Avoid on DCs and clustered roles.** Domain controllers are VSS-sensitive; checkpoints (even production) can cause USN rollback issues. Use proper AD-aware backup instead.