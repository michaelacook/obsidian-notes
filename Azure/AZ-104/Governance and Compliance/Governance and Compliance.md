### Governance and Compliance

#### Managing Subscriptions
- subscriptions - billing unit, aggregate all costs in rgs
- contain resource groups
- scoping level for governance and security
- types of subscriptions:
	- free trial
	- pay as you go
	- etc
- subscription naming conventions
	- prod/dev/staging
		- named based on environment
	- departments/teams
		- e.g, marketing, engineering
	- geographic region
- take aways
	- billing unit
	- contain resource groups & resources
	- scoping level for governance and security, deployment
	- associated with an Entra ID tenant
		- each subscription has a single Azure Entra ID tenant
		- each Entra ID tenant can have multiple subscriptions


#### Using Management Groups
- logical container to manage subscriptions
- place subscriptions inside mgs to organize them
- organizational hierarchy
- provides a scope for enforcing **governance and compliance** and **RBAC**
- parent-child relationship
	- root can't have a group above it
- 6 levels of hierarchy
- compliance support
	- Azure Policies - deploy into management groups
		- filters down to all children
	- Azure role-based access control (RBAC)
		- filters down to all children
- scope
	- use management groups as scope levels for RBAC
	- since scope always filters down to all children, strategically set the scope of RBAC or policy at the correct mg or sub
- no access to root mg by default, must be global admin and elevate to user access admin for the root mg
	- do this if you lost access to a lower mg and need to regain access


#### Understanding Azure Policy
- what is Azure Policy
	- monitor, report and enforce compliance
	- set of rules
	- enforce compliance and enable auditing
	- orgs need to implement enterprise-level governance and compliance capabilities
	- i.e., prohibit specific vm sizes, restrict service access from services we won't use, enforce allowed locations for resource deployment
		- enforce naming conventions
		- tags
		- resource sizes and settings
		- data retention
- components of Azure policies:
	- Policy Definition
		- defines **evaluation criteria** for compliance & **actions** that take place
		- either audit or deny should be something outside of compliance
		- Check Condition - defined in policy definition, checks every hour for compliance
			- e.g., does resource size comply with allowed sizes?
		- Trigger Action - results if Check Condition determines non-compliance
			- prevent, audit, or change to bring into compliance
	- Assign the policy
		- can be assigned to management group, resource, group, subscription, individual resource
	- Initiative Definition
		- definition of policies tailored to a single high-level goal
			- ie, ensuring that VMs meet certain standards



#### Tagging Resources 
- what is resource tagging
	- name:value pair
	- use tags to tag resources according to various criteria
		- e.g. shut down all VMs with a specific tag,
			- devs can only update VMs with a specific tag
				- e.g., if a server has XYZ tag, shut down over the weekend
	- use for cost analysis, target for automation scripts
	- name can be 512 chars, value can be 256 chars
	- tag names for storage accounts can only be 128 chars
- resource tag assignment hierarchy
	- tags not inherited from assignment scope
	- resources don't inherit tags that are set on the rg
	- must tag each resource/rg individually
		- could use Azure Policy to enforce tagging inheritance
	- resources can only have 50 tags at a time
- tags can be edited whereas resource groups cannot


#### Locking and Moving Resources
- What are resource locks
	- override permissions
	- lock subs, rgs and resources
	- restrictions apply to all users regardless of RBAC
- can lock subscriptions, rgs and resources
- can't move a resource from one sub to another unless they are both part of the same Entra ID tenant
- resources are locked during the move
- the resource ID will change after it's moved
- moving resources between subs or rgs doesn't change the resource's deployment location
- not every type of resource can be moved (but many can)
- types of locks
	- **ReadOnly** - authorized users can read, but prevent delete/update operations
	- **CanNotDelete** - authorized users can read and modify, but cannot delete
	- locks are inherited from the parent scope
- moving resources
	- process of moving a resource in one sub, rg, etc to another
	- can move resources between rgs
	- can move resources from sub to sub (sub would need to be associated with same tenant)
	- a read-only lock will prevent a resource from being moved
 

#### Managing Costs
- what impacts costs?
	- subscription type (free, payasyougo,enterprise agreement, CSP)
	- resource type
		- different types of storage and compute
	- usage meters 
		- cpu, ingress/egress traffic, disk size
	- location
		- can depend on region
- cost best practices & tools
	- select appropriate resource for the use case
		- ie., PaaS might be a better solution
	- understand resource sizing
	- deallocate unneeded/unused resources
	- use cloud capabilities where possible
		- scale up when needed, but then scale back in
	- plan costs prior to purchase
		- use cost tools to get quotes first
			- Pricing Calculator
			- TCO calculator
			- Cost Management
				- analyze costs
				- create budgets
- Cost Management + Billing resource provides spending and consumption data as well as the ability to create budget alerts
- [Cost Management Template](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/govern/cost-management/template) - Download the Cost Management discipline template
	- Business Risks
		- identify business risks due to hosting in the cloud
			- over-provisioning - more than you need, would work better in smaller sku
			- under-utilization
			- over-budget
	- Metrics and Indicators
		- get data on usage>indicator to>trigger a warning
	- Policy Compliance Process
		- planning, reviewing, and reporting
		- ongoing monitoring - ensure compliance
		- violations and enforcement actions
	- Toolchain
		- [Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/)
		- [Azure Advisor](https://learn.microsoft.com/en-us/azure/advisor/advisor-cost-recommendations)
		- Cost Management and Billing resource
	- Azure Advisor uses [Action Groups](https://learn.microsoft.com/en-ca/azure/azure-monitor/alerts/action-groups?WT.mc_id=Portal-Microsoft_Azure_Expert)


#### Building a Cloud Governance Strategy
- what is governance?
	- rules, policies and compliance standards for organization
	- control resources, enforce rules and standards
- planning a cloud strategy process:
	- define governance needs for org
		- regulations that must be complied with
		- corporate policies
	- plan which tools will be used
	- ready - understand how tools will be used to implement governance
	- adoption - implement governance for the organization using a cloud strategy
- governance services
	- management groups & subs - organize subs into hierarchical structures
	- role based access control - provide access to resources at various scopes
	- azure policies - implement policies to enforce standards and initiatives
	- locks and tagging - lock resources to prevent deletion, & tag resources to categorize and organize

#### Exam Tips
- Subscriptions are billing units that aggregate costs for all resources under them
- Subscriptions are a scoping level for governance and security
- Resource groups are free
- Multiple resources in multiple regions in the same rg
- RGs are not network boundaries
- Tags are not inherited
- Tags can be used for cost analysis and automation
- Resources are locked while being moved, but will be available
- Resource ID changes after the resource is moved
- Use Pricing Calculator, Azure Advisor and Cost Management and Billing to help manage costs, get alerts, recommendations, and pricing out solutions
- RBAC is supported for management groups
- No access to the Root Management Group by default for security reasons, and it cannot be moved or deleted
- Global Administrators must be elevated to User Access Administrator of the root group to do things in it like assign RBAC


























































