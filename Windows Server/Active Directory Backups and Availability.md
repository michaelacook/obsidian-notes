## Goal
Ensure **availability**, **integrity**, and **recoverability** of authentication services by planning for redundancy, backup, and restore.

## Domain Controller Availability
- AD DS uses **multi-master replication**: all domain controllers (DCs) can accept changes.
- **Minimum recommendation**:
  - At least **2 domain controllers per AD DS site**
  - For enterprises: **2 DCs per geographical region** is the practical minimum
- Benefits:
  - High availability if one DC fails
  - Load distribution during peak authentication times
  - Protection against replication or hardware failures

## Backup and Restore Planning
- Backups alone are not enough; you must understand **how to restore** AD DS.
- AD DS recovery scenarios include:
  - Accidental deletion
  - Database corruption
  - Hardware or OS failure
  - Administrative errors

## Active Directory Recycle Bin
- Designed for **object-level recovery** without downtime.
- Must be **explicitly enabled** (not enabled by default).
- Once enabled:
  - Deleted objects move to the **Deleted Objects** container
  - Objects are retained for the **Deleted Object Lifetime** (default: **180 days** for new forests)
- What is preserved:
  - All object attributes (unlike tombstoned objects)
- Restore options:
  - Restore to **original location**
  - Restore to an **alternate location**

### Limitations
- Cannot undo changes to **existing objects**
  - Example: wrong group membership, attribute changes
- For those cases, you need **traditional backup/restore** methods

## AD DS Backup Requirements
- A valid AD DS restore requires a backup that includes **System State**
- **System State includes**:
  - AD DS database (`ntds.dit`)
  - SYSVOL
  - Registry
  - Boot files and other critical OS components
- Important:
  - A **full server backup used for full server recovery is NOT sufficient** for AD DS restore scenarios

## Directory Services Restore Mode (DSRM)
- AD DS files are **locked while the directory service is running**
- To restore AD DS:
  - DC must be rebooted into **DSRM**
  - AD DS does **not start**, allowing offline access to the database
- Access:
  - Log in using the **local Administrator account**
  - Authenticate with the **DSRM password**
- Restore process:
  - Use **Windows Server Backup** to restore System State
  - Reboot after restore
  - DC reconciles changes via replication

## Nonauthoritative Restore
- **Default restore type**
- Behavior:
  - DC is rolled back to a previous state
  - After restart, it **pulls all newer changes** from replication partners
- Use case:
  - Local database corruption or damage
  - Issue has **not replicated** to other DCs
- Limitation:
  - Cannot recover deleted objects if the deletion already replicated
  - Deleted data will simply replicate back to the restored DC

## Authoritative Restore
- Used when you need to **force restored objects to overwrite newer data**
- Common scenarios:
  - Recovering accidentally deleted OUs, users, or groups
  - Undoing replicated administrative mistakes
- Process:
  1. Perform a **nonauthoritative restore** in DSRM
  2. Before rebooting, mark specific objects as **authoritative**
  3. Restart the DC
- Result:
  - Marked objects have higher version numbers
  - They replicate **outbound** to other DCs, overwriting their copies
