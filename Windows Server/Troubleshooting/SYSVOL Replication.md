- If sysvol replication is not happening, this is likely an issue with DFS replication
- Symptoms of failing replication:
	- GPOs sporadically failing to apply
	- login scripts failing to run
	- expected files in SYSVOL
	- outdated GPOs applying
- Ensure DCs can resolve each other's FQDN, make sure there's no network connectivity issues
- Open Event Viewer on affected domain controllers, Application and Service Logs>DFS Replication. Filter for Critical, Warning, Error
	- Get errors, read and understand, research as needed
- Test sysvol with File Explorer - which direction is replication failing in? Or is it failing in both directions?
- If replication is broken and there are no DNS or connectivity issues, you can perform an authoritative restore. Follow this guide: [SYSVOL Folders Not Replicating Across Domain Controllers](https://thesysadminchannel.com/solved-sysvol-folders-not-replicating-across-domain-controllers/)
	- Before proceeding, backup SYSVOL, GPO, do a snapshot

## Authoritative restore steps:

### 1. Prepare for an authoritative restore
- Decide which DC is up to date (this will be the master, e.g. DC01).
- Ensure you have backups of:
  - GPOs
  - SYSVOL folders on all DCs
- Understand that the master will overwrite SYSVOL on other DCs.

### 2. Stop DFS Replication on the master DC
- On the master DC, open services.msc.
- Stop the DFS Replication service.

### 3. Modify SYSVOL settings on the master DC
- Open ADSI Edit.
- Connect to the Default Naming Context.
- Navigate to:
  Domain → Domain Controllers → Master DC → Domain System Volume → SYSVOL Subscription
- Set:
  - msDFSR-Enabled = FALSE
  - msDFSR-Options = 1

### 4. Modify SYSVOL settings on other DCs
- On each non-master DC, open the same SYSVOL Subscription location.
- Set:
  - msDFSR-Enabled = FALSE
- Leave:
  - msDFSR-Options at the default value 0

### 5. Force Active Directory replication
- Open PowerShell as Administrator on the master DC.
- Run: `repadmin /syncall <MasterDCName> /APed`
- Confirm the command completes with no errors.
  
### 6. Restart DFS Replication on the master DC
- Start the DFS Replication service on the master DC.

### 7. Re-enable SYSVOL replication on the master DC
- In ADSI Edit on the master DC:
  - Set msDFSR-Enabled = TRUE
  - Leave msDFSR-Options = 1
- Force replication again using the same repadmin command.

### 8. Re-enable SYSVOL replication on other DCs
- In ADSI Edit on each non-master DC:
  - Set msDFSR-Enabled = TRUE

### 9. Restart DFS Replication on all DCs
- In PowerShell, use Invoke-Command to stop and start the DFSR service on all DCs.
- Stop the DFSR service.
- Start the DFSR service again.
- `Invoke-Command DC1, DC2, ..., DCn -ScriptBlock { Restart-Service DFSR }`
  
### 10. Verify replication status
- Open Event Viewer → DFS Replication.
- Look for Event ID **4602**, indicating SYSVOL replication has initialized.

### 11. Confirm SYSVOL replication is working
- Create a test file in SYSVOL on DC02 and verify it appears on DC01.
- Create a test file in SYSVOL on DC01 and verify it appears on DC02.
- Confirm replication now works in both directions.
