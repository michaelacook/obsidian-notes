## Regional High Availability and Resiliency

#### Azure Load Balancer
- only works within a region, can work across AZs
- network service to distribute traffic across virtual machines in a **single region**
- operates at layer 4 (transport layer) distributing traffic based on source and destination IP, port number and protocol
- supports availability zones and availability sets for backend virtual machines to provide an SLA of up to **99.99%** when you use 2+ vms
- two SKUs: basic and standard
- supports internal and external load balancing
	- internal - client accessing resource are inside the vnet
	- external - client is outside the vnet and traffic comes from the public Internet
- when to use:
	- increase availability of non-HTTPS apps and services running on VMs in the same region
	- protect against datacenter failure by distributing VMs across AZs
	- provide and control outbound connectivity for VMs in a virtual network

#### Azure Application Gateway
- layer 7 (application layer) network traffic distribution and routing for web applications
- understands HTTP and HTTPS
- can read the hostname and paths of HTTP/S requests
- supports backend pools that include VMs, vmss, Azure App Service and on-premises virtual machines
- supports path-based routing and multi-site hosting
- requires a dedicated subnet that doesn't have other resources deployed to it
	- unlike some other Azure resources that require a dedicated subnet, the subnet doesn't have a required name and CIDR
- can provide additional protection from common threats with Web Application Firewall
- when to use:
	- high availability for web apps within a region
	- to protect against datacenter failures using backend resources that span AZs
	- to better control web traffic based on hostname and address

#### Web Application Firewall
- is a premium tier feature of Application Gateway that provides protection for web applications
- sits between the Internet and the listeners on the Application Gateway
- protects against XSS, SQL Injection and more
- when to use:
	- use the WAF SKU of Application Gateway to protect web applications from common threats such as SQL Injection and XSS


## Global High Availability and Resiliency

#### Azure DNS 
- alows you to host and manage domains using Microsoft's globally distributed infrastructure of name servers
- supports public and private zones
- can be linked to a vnet to provide automatic resource record registration
	- private zones
- 100% uptime SLA
- when to use:
	- use public zones to host and manage your publically accessible DNS zones
	- use private zones to provide hostname to IP resolution with automatic registration in vnets

#### Azure Traffic Manager
- builds upon DNS and is a global DNS-based traffic distribution service
- traffic is based on rules that provide flexibility to direct traffic depending on performance or geography
- supported endpoints include Azure services and endpoints outside of Azure, suvh as on-prem resources or other clouds
- provides extremely high availability with a 99.99% SLA
- when to use:
	- Multiple geographies: to route requests to different endpoints based on geography in order to provide localization and compliance
	- Improve performance: by directing traffic to the closest endpoint
	- Automatic failover: to provide high availability and automatic failover, which may be required in the event of a failure or during maintenance

#### Azure CDN
- provides performance and latency improvements by caching static content (video, audio, images) closer to application users on edge servers
- providers include Microsoft, Akamai, and Verizon
- includes compression and geo-filtering
- when to use:
	- provide better performance and improved user experience when users access large static files
	- reduce the impact on web application services by offloading some requests to cached content

#### Azure Front Door
- provides global high availability and performance improvements for web applications
- combines features of global load balancing, CDN and Web Application Firewall into a single service
- SKUs: performance-focused Standard, security-focused Premium
- when to use:
	- if you have an Internet facing public web application that requires acceleration and the app is globally distributed