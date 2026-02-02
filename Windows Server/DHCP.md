- After installing DHCP, need to authorize the DCHP server
  - Links it to Active Directory
  - This is done because you can set your clients to only accept IPs from authorized servers, so this is to prevent rogue 
DHCP servers on the network
- Green checkmark on IPv4 and IPv6 means it's authorized in AD; red downward arrow means it isn't authorized
  - by default Windows clients will accept IPs from any DHCP server so you'll need a group policy if you want to enforce security around DHCP (probably not a good idea for a workplace where employees have laptops that they take home)
- Red down arrow on a scope means the scope isn't activated
- DHCP configuration involves the creation of DHCP Administrators and DHCP Users groups
- In the root domain, only need to be a domain admin to authorize a DCHP server; in any other domain in the forest, you'll need enterprise admin rights
- DHCP Administrators - manage DHCP
- DHCP Users - read-only
- WINS uses NetBIOS
- Superscope: group scopes together to visualize things intuitively
  - can activate all scopes in a superscope at once by right-clicking the superscope
- To activate a scope you need to be a DHCP Administrator or Domain Administrator

## DHCP Reservations
- when you want to reserve a certain IP for the same device every time
- some times used with printers for instance
- controversial: if DHCP is down, your device isn't getting the IP, better to use a static IP
- Microsoft recommends reservations. My networking prof strongly cautioned against using them for reason stated above
- Specify any reservations under the scope in Reservations
- Don't include reservation IPs in an exclusion range, as the exclusion will override the reservation
- Only static IPs can be in an exclusion range

## DHCP Failover
- [DHCP Failover Settings](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn338985(v=ws.11))
- https://www.youtube.com/watch?v=NaGuUjs6g6o
- https://www.youtube.com/watch?v=S7Eh7ubTVtY
- Install DHCP on the second server, authorize
- On primary server, open DHCP manager
  - right-click a scope and select Configure Failover...
- Failover relationship:
  - Maximum Client Lead Time: how long before the partner server considers the primary server to be down
    - MCLT prevents lease conflicts by limiting how long a DHCP server can hand out or extend leases without coordination, and it controls when a surviving server fully takes over after its partner fails.
  - Mode: 
    - Load balance: split the scope
    - Host standby: partner server waits until primary is down
      - Addresses reserved for standby server: by default the hot spare can renew addresses for clients, but you may also want it to be able to assign new. So this percentage is the IP addresses that only the standby DHCP server can hand out if the primary server is unavailable
  - State Switchover Interval: if primary server goes down, failover would be manual unless you select State Switchover Interval
  - Enable Message Authentication: authenticate heartbeat messages passed back and forth
    - Shared Secret: authenticate with an encrypted shared secret
- Can force replication of scope changes to the partner server immediately by right-clicking a scope and clicking Replicate Scope...
- Can force replication of all scopes with Replicate Relationship...
- DHCP adds rules to Windows Defender Firewall itself needed for DHCP failover
  - DHCP Server Failover (TCP-In)
  - make sure you manually create the rule on any firewalls or routers where applicable