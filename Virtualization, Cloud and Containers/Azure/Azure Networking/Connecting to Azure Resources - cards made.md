#### Public IP Addresses
- [Azure Public IPs](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses)
-  provides inbound connectivity to a resource from the public Internet
- outbound connectivity via a NAT gateway
- supported for VMs, load balancers, VPN gateways and application gateways
- addressing can be dynamic or static
- two skus: Basic and Standard
- when to use:
	- public connectivity
- not recommended to associate a public IP with a virtual machine directly
	- associate with application gateways, load balancers or NAT gateway

#### Azure Bastion
- provides secure remote management for VMs directly from the Azure portal via SSH or RDP
- does not require a public IP on the VM
- supports NSGs
- when to use:
	- to connect to VMs without public IPs
	- to avoid using jumpbox VMs
	- to secure access using network security groups 

#### Azure DDOS Protection
- provides protection from distributed attacks against Azure vnet resources that are exposed via a public IP
- basic protection is provided at no additional cost
- can upgrade from basic to DDOS Standard
	- cost guarantee
	- rapid response team
	- enabled per org, protects up to 100 resources
- when to use DDOS Standard:
	- protect critical websites and applications
	- protect against the costs of these attacks
	- access to a dedicated response team at Microsoft