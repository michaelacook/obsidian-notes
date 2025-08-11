#### Virtual Network Gateway
- a generic service that provides VPN or ExpressRoute connectivity to Azure
- requires a subnet called GatewaySubnet with a CIDR of at least /27
- can support either ExpressRoute or VPN connection, but not both on the same gateway

#### VPN Gateway
- connectivity between on-prem networks, other clouds, and remote devices over the Internet using a VPN
- supports site-to-site, multisite, and point-to-site connection types
- supports route-based VPN connections, as well as policy-based connections for legacy devices

#### ExpressRoute
- direct private connection between on-prem and Azure
- can be private, or a Microsoft peering connection
	- private - between remote network and Azure vnet
	- peering - on-prem and Microsoft cloud (allows M365 access)
- supports three connection types:
	- IPVPN - integrate Multiprotocol Label Switching (MPLS)
	- Point-to-point Ethernet to connect directly between on-prem and Azure
	- CloudExchange to connect from co-locations
- VPN Gateway vs. ExpressRoute
	- VPN Gateway
		- used to connect on-prem networks, other clouds, and remote workers
		- connects only to virtual networks, not the greater Microsoft cloud
		- connects via the public Internet
	- ExpressRoute
		- used to connect on-prem networks to the cloud
		- connects to virtual networks and the Microsoft cloud
		- is a dedicated, private connection that does not traverse the public Internet

#### Azure Virtual WAN
- a managed hub and spoke topology that provides connectivity between virtual networks, on-prem networks, and mobile workers
- uses one regional hub per Azure region, and connected spokes within that region
- regional hubs are connected together automatically to support more regions and larger global networks
- when to use:
	- to manage large regional and global networks with multiple virtual networks, on-prem networks and remote devices/workers
	- replace MPLS and integrate SD-WAN
	- improve performance of regional and global networks by leveraging the high-speed Azure backbone network to connect between networks