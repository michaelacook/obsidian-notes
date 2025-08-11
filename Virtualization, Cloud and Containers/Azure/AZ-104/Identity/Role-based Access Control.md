#### Understanding Roles in Azure
- **Who** can do **what**, **where**?
- **Security principals** can have **role assignments** and at a **scope**
- two different kinds of roles
	- Azure roles (for resources in Azure subscriptions)
		- provide identity objects in tenant with a role assignment with a Role Definition
		- then determine scope for the Role Definition - can be at different scopes
		- role assignments are inherited from parent scope - waterfall
		- can create custom roles
		- main roles:
			- **Owner** - full access to resources and delegate to other users
			- **Reader** - view resources, can't modify or delete
			- **Contributor** - create and manage resources, can't manage access for others
			- **User Access Administrator** - ability to manage user access, but not resources
	- Entra ID roles (for managing identities in Entra ID)
		- special set of roles for providing access to manage identity objects in the tenant itself, not in subscriptions
		- manage users, applications, devices
		- make a Role Assignment to allow an admin to peWhatrform specific actions on other identity resources in the tenant
		- can create custom roles
		- scope is at the tenant level or for an Administrative Unit 
			- Administrative Units provide the ability to introduce pseudo-hierarchy within the flat structure of Entra ID for scoping Entra ID roles
		- main roles:
			- **Global Administrator** - manage Entra ID resources - root level admin
			- **Billing Administrator** - billing tasks
			- **User Administrator** - manage users and groups
			- **Helpdesk Administrator** - helpdesk functions such as password resets

#### Assigning Roles
- RBAC is an authorization system
	- Security principal (who?)
		- User or group
	- Role definition (what?)
		- Assign role definition to a security principal
	- Scope (where?)
- Identity objects have a default implicit deny - can't do anything unless they're granted authorization
- access must be explicitly granted with a role assignment at a scope
- role definition:

```json
"Actions:" [
	"*"
],
"NotActions": [
	"Auth/*/Delete",
	...
],
"DataAction": [],
"NotDataActions": [],
"AssignableScopes": [
	"/"
]
```

- control plane - managing resources in Azure
- data plane - data, e.g., in a storage account
- Collective Permissions = Actions - NotActions
- Overlapping roles
	- roles are addititve 
	- Effective Permissions = role + role
	- most permissive wins
