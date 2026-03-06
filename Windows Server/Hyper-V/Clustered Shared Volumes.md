# Cluster Shared Volumes (CSV)

A CSV is a shared disk resource in a Windows Server Failover Cluster that allows multiple cluster nodes to simultaneously have read/write access to the same NTFS or ReFS volume.

## The Problem CSVs Solve

Without CSVs, a traditional failover cluster disk can only be **owned by one node at a time**. CSVs remove that limitation by using a coordinated metadata system — one node acts as the **coordinator** (metadata owner) for the volume, while all other nodes can still perform direct I/O to the storage through redirected or direct access paths.

## Key Points

- **Primary use case:** Most commonly associated with Hyper-V, where they allow VMs on different hosts to share the same underlying storage without needing separate LUNs per host.
- **Default path:** CSVs live under `C:\ClusterStorage\` by default.
- **Resilient I/O:** If the coordinator node loses connectivity to the storage, I/O from other nodes gets transparently redirected over the cluster network rather than failing outright.
- **Simplifies live migration:** Storage doesn't need to "move" between nodes during a live migration — every node already has access.

## One-Liner

CSVs solve the "one owner at a time" problem of clustered disks by letting all nodes in the cluster access the same volume concurrently.