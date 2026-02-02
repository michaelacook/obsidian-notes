### Vnets
- private network defined by a private IP address space
- can't span multiple subscriptions
- cannot span a region
- communication boundary - by default anything within a vnet can communicate with everything else in that network, but not outside of it
### Subnets
- segment of a virtual network
- IP range is a subset of the vnet address space
- multiple subnets to segment a virtual network
	- improves security because a lot of security measures work at the subnet level
	- improve manageability, more efficient IP usage
	- different devices for different purposes
- security boundary
- some Azure resources require a dedicated subnet
	- Azure Bastion
	- SQL Managed Instance
	- Virtual Network Gateway
### Network Interfaces
- resources connect to a network with a nic
- a network interface is used to configure IP addresses and DNS settings
- can have multiple private IPs and configurations
- mostly on VMs, which must have at least one nic
### Private IPs
- a private IP provides the IP config for a nic to connect to a subnet in a vnet
- can have multiple IP configurations, each with a private IP
- can be primary or secondary
	- primary can be static or dynamic (DHCP)
	- can have a public IP as well
- all resources deployed to a vnet must have a private IP
- when to use a private IP?
	- Hosting secure websites
		- If a virtual machine hosts multiple websites with different SSL certificates, you might assign multiple IP addresses to a single network interface
	- Deploying virtual appliances
		- Virtual appliances like load balancers and firewalls might use multiple IP addresses to provide different frontend configurations within those appliances
### Internet Connectivity
- Internet access by default via a temporary public IP and NAT on the vnet
	- this is more of a convenience feature and not meant to be the configuration you keep in many scenarios
	- that default public IP is dynamically assigned through DHCP
	- this isn't great for situations where more control is required
		- e.g., you need the public IP on the frontend of the NAT gateway to stay the same for various reasons
### NAT Gateways
- What is a NAT gateway?
	- A network address translation gateway provides Internet access through a single static public IP address or a pool of them
		- This is a good replacement for the default Internet access Azure provides to vnets that allows for more control
	- can be linked to one or more subnets
	- use when you want more control over Internet access from VMs