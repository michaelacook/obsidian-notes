What is disaster recover
- Recover from any sort of disaster that affects availability of IT services and applications
- Every business needs a business continuity and disaster recovery plan (BCDR)
- Need to assess physical, climactic, political risks
- Determine critical workloads
- What is the technique/strategy for recovery is
- Test disaster recovery

RPO vs. RTO
- **Recovery Time Objective (RTO)**
	- amount of time from the disruptive event to when resources must be fully available again
	- how much loss in time the business/organization can suffer if a resource is unavailable
	- how long will it take to recover
	- downtime
	- hours
- **Recovery Point Objective (RPO)**
	- maximum amount of data that can be lost before significant impact to the business/org
	- how much data can you tolerate losing
	- what can we stand to lose?
	- data lost
	- if org has low tolerance for data loss, need frequent snapshots or backups
	- high tolerance for data loss, less frequent snapshots/backups
- [What is the difference between RPO and RTRO?](https://www.acronis.com/en-us/blog/posts/rto-rpo/)
- All about meeting needs for business continuity

Disaster recovery methods
- Backup business critical data
- Cold site - a copy of critical infrastructure that needs preparation before disaster recovery is complete
	- when you have a high RTO?
	- Infrastructure as Code?
- Hot site - a copy of critical infrastructure and data that is ready to be swapped in as the production workload in the event of a disaster
	- when you have a low RTO?
	- hot-swappable infrastructure waiting for failover?