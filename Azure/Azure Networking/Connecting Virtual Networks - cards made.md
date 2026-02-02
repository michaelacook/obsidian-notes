#### VNet Peering
- peering is when two networks are joined to allow traffic between them
- can connect across subscriptions and regions
- when to use: 
	- connect separate vnets 
	- hub and spoke topology
		- Azure best-practice
- tips:
	- peering must be created in both directions
	- non-transitive
	- address space must not overlap
		- e.g., two networks with an address space of 10.0.0.0/16 cannot be peered
			- instead, 10.0.0.0/16 and 10.1.0.0/16 can be peered

#### Default Azure Routes

| Address Prefix     | Next Hop        | Description                                           |
| ------------------ | --------------- | ----------------------------------------------------- |
| Vnet address range | Virtual network | Routes vnet traffic within the virtual network        |
| 0.0.0.0/0          | Internet        | Default route to the internet                         |
| 10.0.0.0/8         | None            | Drops private IP ranges that are not part of the vnet |
| 172.16.0.0/12      | None            | Drops private IP ranges that are not part of the vnet |
| 192.168.0.0/16     | None            | Drops private IP ranges that are not part of the vnet |
| 100.64.0.0/10      | None            | Drops shared address space traffic. ISP IP range      |

#### Custom Routes
- allow you to control the flow of traffic in the vnet
- two types:
	- User-defined routes
		- take precedence over all other kinds of routes
	- Border Gateway Protocol (BGP)
- when to use:
	- route traffic through a network virtual appliance
	- route traffic to an on-premises network
