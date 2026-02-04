Active Directory Domain Trusts
- Trust relationship: connection between domains that allows sharing of resources
- automatic trust relationship between domains called two-way transitive trust
  - Transitive: if domain A has a trust with B and B has a trust with C, A trusts C
- One-way directional trust: domain B trusts domain A, but not the reverse
  - this allows domain A to access resources in domain B
- Shortcut trust: for slow WAN connections, authentication/connection is slow due to transitive trust
  - authentication goes all the way up to the domain tree to the root and then back down
  - shortcut trust sets up a transitive trust between two domains that aren't part of the same domain tree so that the authentication doesn't have to go all the way up to the root domain and then across and down
- Realm trust: UNIX Realm (Kerberos)
  - transitive or one-way
  - setting up a trust with UNIX machines (BSD, Linux, Solaris, etc.)
- Forest trust: trust relationship between two forests
  - merger/acquisition
  - DNS pointing back and forth
  - WAN connection
  - two-way or one-way trust between the root domains of each forest

## Setting up trust relationships
- Active Directory Domains and Trusts
- Must have a WAN connection of some kind and DNS
- DNS>Conditional Forwarders
  - create a conditional forwarder for the other domain on each side of the trust
- Then in AD Domains and Trusts, right-click domain, Properties>Trusts
  - New Trust, follow prompts in the wizard
  - must be done on both sides of the trust

## Sites and Replication
- Site: object in AD that represents a physical location, used to help control replication between DCs
- Default first site: everything is here by default unless you put DCs in another site
- Ovals usually represent sites in AD
- KCC: Knowledge Consistency Checker 
  - on each in a site DC, communicate with each other to determine latency, and form a ring of replication amongst each other if they're close
  - intra-site replication
    - replicate within 30 seconds of a change
  - every 15 mins, checks connectivity and updates the topology if a DC goes down
- For physically disparate DCs, you need to create new sites and move the DCs to the correct site
  - if you don't, they will try to replicate with DCs in another physical location which may be very slow depending on your WAN links
- by default, in each site the DCs will elect a Bridge Head server that will do intersite replication once every 180 minutes
  - this is better for replication over slow WAN links
- Site Link Bridging: each 180 minutes all Bridge Head servers talk for intersite replication
  - by default every BH server can talk to every BH server
  - in a very slow environment, may want to disable this for some slower sites so that they only ever replicate with their parent
- Can assign a name and cost to a site link to control the direction of replication
  - default cost is 100
    - make it higher or lower to change direction of replication
    - faster connection means lower cost
- Must associate TCP/IP subnets with each site

## Working with AD Replication
- Test and monitor replication
- Force replication between DCs: AD Sites and Services, go to site>server>NTDS settings and right-click>choose a replication option
- Can also force replication from the command prompt
  - see replication connections: `repadmin /showrepl`
  - force replication: repadmin /replicate
  - help: `repadmin /?`
  - `dcdiag.exe` - Active Directory diagnostics
  - `dcdiag.exe /fix`
  - `dcdiag.exe` > location to read report

### Enable global catalog
- AD Sites and Services>Site>Server>NTDS Settings>Properties>check global catalog











