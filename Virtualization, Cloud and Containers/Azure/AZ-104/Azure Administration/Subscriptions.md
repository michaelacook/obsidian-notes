### Region Pairs
- **Usually at least 300 miles apart**: though not an absolute - reduces likelihood of a disaster that affects both regions
- **Platform-provided replication**: some services like GRS storage provide automatic replication to the paired region
- **Region recovery order**: one region is always prioritized for recovery; apps deployed across paired regions are guaranteed to have one of the regions recovered wit priority
- **Sequential updates**: Azure system updates are rolled out sequentially to minimize downtime and bugs, logical failures (rare)
- **Date residency**: regions reside within the same geography as their enabled set (except for the Brazil South and Singapore regions)
- Use Azure global infrastructure website to find supported regions for your business geography

##### Considerations for regional and region-pair deployments
- Deploy to the physical geography where you want the service or app to be available
- Not all services or VM features or storage options are available in all regions
- Some services don't need region support - they are global (e.g., Entra ID, Azure Traffic Manager, Azure DNS)
- Some regions are paired with another region outside their physical geography (Brazil South, Singapore)
- Take advantage of the benefits of data residency offered by regional pairs. This feature can help you meet requirements for tax and law enforcement jurisdiction purposes

### Implement Azure subscriptions
- **Subscription**: a logical unit of Azure services linked to an Azure account
- **Account**: an identity in Entra ID or a directory that's trusted by Entra ID
- Can have multiple subscriptions linked to the same account
- More than one account can be linked to the same subscription
- Programmatic operations for cloud services may require the subscription ID
- Use a shared services subscription to ensure all common network resources are billed together and isolated from other workloads (e.g., ExpressRoute and Virtual WAN)

#### Subscription procurement options
- **Enterprise agreement**: upfront monetary commitment
- **Microsoft reseller**: get Azure through an Open Licensing program; flexible, simple way to purchase cloud services from a reseller
- **Microsoft partner**: can design and implement an Azure solution for your organization
- **Personal free account**: free trial; no upfront cost

##### Types of subscriptions
- Free
- Pay as you go
- Enterprise Agreement
- Student
###### Considerations
- Start with Free
	- Requires phone number, credit card, Microsoft account
- Use pay as you go for monthly billing - individuals and SMB
- Enterprise Agreement is good for large organizations and allows purchasing cloud services and licenses under one agreement
	- Comes with discounts on new licenses and Software Assurance
- Student subscription - free services without providing a credit card during sign-up
	- Must verify student status through institution-provided email address

### Implement Microsoft Cost Management
- Cost Management provides support for administrative billing tasks, helps manage billing access to costs
- monitor and control Azure spending, optimize resource usage
- get recommendations for cost savings
- optimize resource usage for cost
- export data and store on-prem or in Azure Files

### Apply cost savings
- **Reservations**: paying ahead saves money; 1 or 3 year reservations on VM, SQL, CosmosDB and some other resources can result in up to 72% savings over pay as you go
- **Azure Hybrid Benefits**: pricing benefits if you have a license that includes *Software Assurance*; maximize value of existing on-prem Windows Server or SQL Server licenses when migrating to Azure; use Azure Hybrid Benefit Savings Calculator
- **Azure Credits**: monthly credit beneift for development, testing and experimentation; for Visual Studio subscribers; make Azure your sandbox env
- **Azure Regions**: compare pricing across regions to save costs
- **Budgets**: use budgeting features in Cost Management to help plan and drive accountability; monitor spending over time
- **Pricing Calculator**: provides estimates of spend in all resource areas: compute, networking, storage, web, database