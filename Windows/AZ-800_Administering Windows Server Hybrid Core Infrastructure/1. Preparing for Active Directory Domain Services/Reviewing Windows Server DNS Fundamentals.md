### DNS Basics
- Important to read:
	- [How DNS Works](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd197446(v=ws.10))
	- [Managing a DNS Server](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816758(v=ws.10))
	- [Managing a Forward Lookup Zone](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816891(v=ws.10))
- Each domain is a zone
- The authoritative server for a zone is called a name server
- DNS forwarders
	- server forwarders - catch-all forwarded, used if no conditional forwarder
	- conditional forwarders - forwarder for a specific domain
- Root hints - addresses for the root name servers, used if the DNS server can't find a record in its cache and forwarders fail to result in a response
- Forward lookup zone - hostnames to IP
- Reverse lookup zone - IP to hostname
- Three kinds of zones, each can be either forward and reverse lookup zones:
	- Primary zone - writable copy of the zone
	- Secondary zone - read-only copy replicated from a primary name server
	- Stub zone - stores name server and Start of Authority (SOA) information for another zone
		- SOA - stuff like name servers for the zone
- Zone storage
	- Standard zone - a zone can be stored in a .dns file on a name server
	- Active Directory-integrated - stored in the AD database, requires a domain controller and cannot be a secondary zone because they are read/write

------
### Managing Windows Server DNS
- Resource records
	- A/AAAA - maps FQDN to IP
	- PTR - IP to FQDN; reverse lookup zone
	- SOA - stores administrative information about the zone - NS servers, TTL
	- NS - stores the IP address of all servers that store the zone
	- CNAME - also referred to as an alias; maps a FQDN to another FQDN
	- MX - stores info about email servers for a domain
	- SRV - stores IP and port info for servers that host services in the domain; used extensively by ADDS
	- TXT - stores text-based info about a host or domain name; used for domain validation scenarios
- Recursive DNS Servers
	- A DNS server will be authoritative for it's own domain and recursive for all others, meaning it would forward the request
	- different ways to handle recursive lookups:
		- Conditional forwarder - forward requests for a specific zone to a static list of name servers
		- Stub zone - forward requests for a specific zone to a dynamic list of name servers
		- Server forwarder - forward all unknown requests to a static list of DNS servers
		- Root hints - forward all unknown requests to a list of root name servers

----
### Implementing DNS Security Extensions (DNSSEC)
- Why do we need it?
	- Chain of Trust
	- if the DNS server is not authoritative, it forwards the request (recursive)
	- an attacker could respond to the client instead of the authoritative name server, or an attacker could respond to the recursive DNS server
- DNSSEC allows DNS servers and clients to verify the authenticity of responses using digital signatures to ensure the response hasn't been tampered with
- Components
	- Signed DNS zone - authoritative server signs the zone
	- Trust Anchors/trust points - known public keys distributed to other servers that aren't authoritative for a zone (recursive) so they can validate the authenticity of DNS responses for a zone
	- Name Resolution Policy Table (NRPT) rules - configured on DNS client, defines which zones should be secured
- Distributing trust anchors on DNS servers that are also DCs is easier, distributed through Active Directory
- After signing a zone, the trust anchors are stored in `C:\Windows\System32\dns\keyset-zonename`
- `Resolve-DNSName domain -dnssecok` - this resolves the name and tells the DNS server that the client is DNSSEC capable
- Steps to configure:
	1. Sign the zone on the authoritative server
		1. DNS management console>Forward lookup zone>DNSSEC>Sign the Zone
		2. Use default settings to sign
		3. Next, next, finish
	2. Trust Anchors in `C:\Windows\System32\dns\keyset-zonename`, copy keyset trust anchor and place somewhere in the filesystem of the non-authoritative DNS server
	3. On non-authoritative server go to DNS console>Trust Points>Import>DNSKEY and locate the trust anchor file
	4. Configure Name Resolution Policy Table on DNS client
		1. Can do this with Active Directory to distribute group policy to the client
		2. If not domain-joined, use Local Group Policy Editor
			1. Computer Configuration>Windows Settings>Name Resolution Policy
			2. Add a rule. Provide DNS suffix which is the zone
			3. Enable DNSSEC for the rule
			4. Require DNS clients to check that name and address data has been validated by the DNS server
			5. Apply
	5. Confirm rule on DNS client with `Get-DnsClientNrptRule`
