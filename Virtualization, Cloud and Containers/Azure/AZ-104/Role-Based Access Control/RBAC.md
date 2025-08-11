Understanding roles in azure
-describe rbac
	-who can do what, and where
		-who: security principals
		-what: roles
		-where: scope
-describe azure roles
	-provide role assignment with a role definition to a security principal, with a scope
	-azure roles are for resources inside azure subscriptions
	-main roles:
		-owner - full access and delegate access to others
		-reader - view only
		-contributor - create and manage, but not manage access
		-user access administrator - manage user access, but not manage resources
-describe azure AD roles
	-special set of roles just for providing access identity objects in azure AD tenant (not subscription)
	-global administrator - manage azure AD resources entirely
	-billing administrator - billing tasks
	-user administrator - manage users and groups
	-help desk administrator - helpdesk functions like password resets
-azure vs azure AD roles
	-managing access to resources vs managing identities with scope at tenant level
	-can create custom azure roles
-rbac architecture
	-can provide the global admin with user access administrator roles so it can be the root of the tenant and all the azure resources
		-locked out of global admin and recover by using a backup global admin, regain access of org
			-never leave as default, risky
-----------------------------------------------------------------------------------------------------------------------------

Assigning Access to Resources
-Explaining Azure RBAC
	-authorization system
		-Security principal (who?)
		-Role definition (what can they do?)
		-Scope (where can they perform actions?)
	-identity objects have a default deny - can't do anything anywhere unless given a role assignment
	-explicit deny assignments can override role assignment
		-ie., can access all resources except the one where we have denied
-Understanding Role Definitions
		-Actions - what can be performed on the management plane
		-NotActions - exclusions/deny actions
		-DataAction, NotDataActions - working with data, not on management plane
		-AssignableScopes - scope of definition (i.e., sub, mg, etc)
	-Actions minus NotActions = Collective Permissions
-Additive Properties and Effective Permissions
	-security principal can be assigned more than one role, and they have different permissions
	-roles follow an additive scheme
	-effective permissions is the sum of all permissions
	-Least permissive wins
------------------------------------------------------------------------------------------------------------

Creating Custom Roles
	-no different from built-in roles except you define them
	-need User Access Administrator or Owner role for the account
-Describe Custom Roles
-Creating Role Definitions









































































