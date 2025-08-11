- create, assign and manage policies to control or audit resources
- ensure resources are deployed properly according to org standards
- runs evaluations and scans on your resources to make sure they're complaint

### Create Management Groups
- mangement groups - containers to manage access, policy, and compliance across subscriptions
- key points:
	- all subscriptions placed under the root management group by default
	- all subscriptions inherit the conditions applied to the parent management group
	- supports up to six levels of depth
	- Azure RBAC access control authorization for mangement groups isn't enabled by default - can't do stuff to management groups by default
- Things to consider:
	- use custom hierarchies to meet organizational structure and business requirements/scenarios
	- consider inheritance of policies - place policies at the correct levels to ensure rules are enforced where they need to be
	- consider compliance rules - use management groups to create the hierarchy needed to enforce rules for individual departments and teams
	- consider cost reporting - use management groups to do cost reporting by department
- mangement groups have a directory **unique identifier (ID)** and a **display name**
	- ID used to submit commands on the management group
	- ID can't be changed
	- display name is optional and can be changed

### Implement Azure policies
- Advantages:
	- use built-in policies or make custom policies; real-time policy evaluation and enforcement, or periodic or on-demand complaince evaluation
	- apply at any scale required; aggregate policy state with policy initiatives; define exclusions
	- conduct real-time remediation
	- exercise governance: standardize and enforce configurations; manage multiple subscriptions; support multiple teams; manage regulatory compliance, cost control, security, and design consistency
- Things to consider:
	- consider deployable resources: limit skus, types of resources, etc
	- consider location restrictions: limit locations for deploying resources to locations that make sense for your organization
	- consider rules enforcement: enforce compliance rules and configuration options to help manage resources an duser options, e.g., enforce tagging
	- consider inventory audits: use Azure Policy with Backup and run inventory audits

### Create Azure policies
- A policy definition describes the compliance conditions for a resource and the actions to complete when the conditions are met
	- eg., when resources at scope A meet condition X, perform action Y
- group policy definitions into an initiative definition
- Four steps to create a work with policy definitions:
	1. Create policy definition(s)
	2. Create an initiative definition
	3. Scope the initiative definition
	4. Determine compliance

### Create policy definitions
- view built-in policy definitions in Policy>Authoring>Definitions
	- filter based on category
- add new policy definitions that aren't built-in
	- specify a location (scope)
	- name
	- description
	- category
		- new or existing
	- policy rule in JSON
	- can import policy definitions from GitHub
- make sure to use the [correct JSON structure Azure requires](https://learn.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure-basics)

### Create an initiative definition
- use case: initiative to ensure resources are compliant with all security regulations for your industry
- Microsoft recommends putting policy definitions into an initiative definition even if you only have a few policies
- make sure to use the [correct initiative definition JSON structure](https://learn.microsoft.com/en-us/azure/governance/policy/concepts/initiative-definition-structure) required by Azure
- view initiative definitions in the same location as policy definitions
- can create your own or use built-in

### Scope initiative definition
- after creating initiative definition, assign it to a scope

### Determine compliance
- last step after creating and enforcing policy definition(s) or initiative

