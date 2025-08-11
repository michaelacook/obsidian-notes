### Part 1: NSGs and ASGs

- [NSGs and ASGs YouTube Video](https://www.youtube.com/watch?v=w8H5fWBHddA)
#### Network Security Groups (NSG)
- filter inbound and outbound traffic
- applied to subnets or nics
- can be applied to multiple nics or subnets
- filter traffic based on a priority list of rules
	- IP address
	- source port
	- dest port
	- protocol
- Service tags and services
	- simplify creation
	- service tags - network destinations predefined by Microsoft
	- services - port and protocol combos
- Use when:
	- need to prevent resources from communicating with unwanted destinations
	- need to prevent unwanted traffic from communicating with a subnet or resource
	- good to link NSGs to subnets rather than nics whenever possible to reduce administrative effort

#### Application Security Groups (ASG)
- [Application security groups](https://learn.microsoft.com/en-us/azure/virtual-network/application-security-groups)
- Just a definition, it has no rules associated with it
- group network interfaces of similar virtual machines to apply rules for all of them
- apply a common set of network security group rules to all virtual machines in an application security group
	- You want a rule specified in an NSG to apply a logical grouping of resources rather than to a whole subnet or even to just a single subnet
- sort of like defining your own service tags
- associate multiple resources to an ASG by their nic, then use the ASG as the destination in an NSG rule
- Allow you to apply NSG rules to a similar group of VMs or resources across different subnets

### Part 2: Azure Firewall and Network Virtual Appliances

#### Azure Firewall
- managed firewall service
- sits between networks
- blocks all traffic by default
- network traffic controlled based on rules
- protection between external and internal networks or between multiple internal networks, or between virtual and on-prem networks
- Use when:
	- provide monitoring and protection between networks
	- for Defense In-depth
	- meet compliance requirements
		- some companies may be required to have a firewall between trusted and untrusted networks

#### Network Virtual Appliance (NVA)
- a VM that provides advanced networking capabilities
	- e.g., firewall, application delivery controllers (ADC), load balancers
- bring your own firewall or other appliance for consistent vendor across on-prem and cloud
- you are responsible for it, it's not a managed instance
- When to use:
	- when you need advanced network capabilities not natively available in Azure
	- maintain a single vendor approach across on-prem and cloud
	- to leverage existing skills in your organization
	- to bring your own licenses

### Part 3: Service Endpoints, Azure Private Link, and Private Endpoints

#### Service Endpoints
- by default, a VM accessing a PaaS resource will send that traffic through the public Internet to access the service
- service endpoints allow direct connectivity from vnets to PaaS services so that communication between them does not go over the public Internet
- allows preventing public access and allowing access only from designated networks
- supports Azure Storage, Azure SQL Database, Azure Key Vault, etc
- doesn't remove the public endpoint, but allows you to have connectivity without using it
- for storage accounts, the endpoint is available for the primary and secondary region for a geography but otherwise not available outside that geography
- when to use:
	- restrict access to PaaS service to specific subnets
	- reduce attach surface on PaaS offering

#### Azure Private Link
- provides private connectivity to PaaS services by removing the public endpoint
- glue between vnets and PaaS resources - is its own resource
- uses a private endpoint
- supported by many Azure PaaS services
- works on connected virtual networks, including globally peered networks
- inbound and outbound data is billable
- can be integrated with DNS
- uses a private endpoint
- when to use:
	- to provide direct connectivity to a PaaS offering from a vnet and eliminate the public endpoints
	- enable access across regions or from your on-prem networks

#### Private Endpoint
- a network interface deployed to a virtual network to provide connectivity from that virtual network to the instance of the PaaS offering that it is linked to

#### Service Endpoint vs. Private Endpoint
- service endpoint 
	- connectivity from a subnet to an entire PaaS service
	- some regional limitations
	- cannot be used by on-prem networks
	- uses routes to direct traffic
- private endpoint
	- provides connectivity to a single instance of an Azure service
	- uses a network interface
	- supports connected networks, on-prem networks
	- integrates with DNS
	- data transfer is billable