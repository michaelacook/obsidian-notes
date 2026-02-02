
---
This is a set of notes ChatGPT made for me when snapshots of a container that were supposed to be saved to an NFS share were failing.

The failure is coming from tar reading your **temporary backup tree on the NFS share**. Because the tmp dir lives on NFS, root is getting squashed (or ACL/xattr semantics aren’t available), so tar can’t read some paths (rsyslog, postfix, logs). It then exits with status 2 and vzdump marks the job as failed.

You already saw the hint in the log:

> “temporary directory is on NFS … consider configuring a local tmpdir via /etc/vzdump.conf”
### 1. Format & mount the new disk

If it’s blank and unformatted:

```bash
# Find the device name
lsblk

# Assuming it's /dev/sdb — adjust as needed
mkfs.ext4 /dev/sdb

# Create a mount point
mkdir -p /mnt/vzdump-tmp

# Mount it
mount /dev/sdb /mnt/vzdump-tmp
```

---

### 2. Make it mount automatically at boot

Edit `/etc/fstab` and add:

```fstab
/dev/sdb    /mnt/vzdump-tmp    ext4    defaults    0    2
```

Then test:

```bash
mount -a
```

---

### 3. Set correct permissions

```bash
chown root:root /mnt/vzdump-tmp
chmod 700 /mnt/vzdump-tmp
```

---

### 4. Tell vzdump to use it

Edit `/etc/vzdump.conf`:

```conf
tmpdir: /mnt/vzdump-tmp
```

---

### 5. Test the backup

```bash
vzdump 100 --storage backups-nfs --compress zstd --remove 0
```

Now Proxmox will do all temporary work on your new 32 GB disk, and only write the final `.tar.zst` file to your NFS share — no more root-squash permission issues during tar.

---
