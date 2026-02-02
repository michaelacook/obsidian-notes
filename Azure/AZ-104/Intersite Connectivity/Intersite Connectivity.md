Vnet Peering

Describe vnet peering
-by default, separate vnets are isolated - this is for security
-vnet peering is needed to bridge together vnets so resources can talk to each other
-vnet peering is non-reciprocal - connection must be established on both sides
-can peer within and across azure regions
-peerings are non-transitive

Benefits
-low-latency, high-bandwidth connections - traffic between resources doesn't have to go over public internet, it traverses the azure backbone network instead
-cross-network communications
-data transfer between/across:
	-subscriptions
	-Entra ID tenants
	-regions

Implementation
- IP CIDRs of different vnets can't overlap
- select Peerings in left pane menu to peer a network
------------------------------------------------------------------------------------

Implementing VPNs

VPN Gateways vs VNet Peering
-same purposes as vnet peering
-use for peering an on-premises network to a virtual network
-use a VPN gateway with a gateway subnet and a public IP per vnet gateway
-uses an IPsec tunnel for encryption

Capabilities of VPN Gateways
-create point to site and site to site connections over the public internet
-have transitive traffic unlike vnet peering

Routing Types & Gateway SKUs
-policy-based
-route-based

Active-Active vs Active-Passive Configuration
-see image

Implementation
- gateway subnet must be called GatewaySubnet
	- recommended to use a /27 network for this
- see implementation steps image
------------------------------------------------------------------------------------

ExpressRoute

Describe ExpressRoute
-makes a direct physical connection into Azure
-not encrypted by default
-totally private connection to Azure, Microsoft 365
-Microsoft edge location - partner edge location (ISP)
------------------------------------------------------------------------------------

Implementing Virtual WAN
-SDWAN
-hub and spoke architecture

What is Virtual WAN
-Azure WAN
-service to create a single operational interface to manage a hub and spoke or fully meshed network
-create WAN hubs

Hub-to-Hub Connectivity
-can create a hub in different regions then create connections between hubs in these regions to create a global, meshed network

Virtual WAN SKUs
-two SKUs:
	-basic
		-no transitive peering
		-S2S VPN connection only
		-supports upgrading to Standard SKU
	-standard
		-transitive
		-S2S VPN, P2S VPN, ExpressRoute, and VNet-to-VNet connections














































