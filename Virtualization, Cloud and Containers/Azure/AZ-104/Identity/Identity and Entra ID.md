#### Conceptualizing Azure Active Directory
- identity and access management basics
	- identity-centric security
	- identity profile to authenticate, assume identity to gain certain authorizations
	- identities can have role assignments that provide them permissions/privileges to perform certain actions at certain scopes
- what is Entra ID?
	- identity and access management solution
	- provides an identity repository
	- identity resources 
		- users 
		- groups
	- enables the management of identity security
		- MFA
		- control resource access
		- provide policy-based controls
- Entra tenant architecture
	- global service that spans the cloud
	- specific instance is scoped to a geography, one you're in when you create it
	- get a default domain name, e.g, prefix.onmicrosoft.com
	- can register a custom domain with a txt or cname record in DNS
	- identity resources in entra ID need access to perform certain actions on resources in Azure
		- role assignments for subscriptions that have a trust relationship with the AD tenant
		- tenants can have a trust relationship with many subs, subs with only one tenant
- Entra ID features
	- IAM platform for Azure cloud-based resources
	- Security: MFA and conditional access, and Privileged Identity Management - escalate privileges for JIT access, Conditional Access policies
	- collaboration and development - B2B for collaboration between businesses and B2C to support development
	- Monitoring - audit logs, security monitoring, identity protection, and risk management
	- hybrid infrastructure for single sign-on, identity integration with Microsoft Entra Connect
	- enterprise access - additional security for applications and devices both on-premises and in the cloud
	- Identity Governance - feature that allows us to perform reviews of who really needs what privileges so what we might critique things to be more secure or more efficient
- Licensing
	- [Microsoft Entra ID Licensing](https://learn.microsoft.com/en-us/entra/fundamentals/licensing)
	- Tiers:
		- Free Tier
			- unlimited single sign-on
			- cloud and federated authentication
			- self-service password reset
			- basic MFA
		- Premium P1
			- Advanced MFA
			- Advanced group management
			- Conditional access policies
			- User and group provisioning to applications
		- Premium P2
			- Risk-based conditional access
			- Privileged identity management (PIM)
			- Basic access reviews
			- Basic entitlement management
		- Governance add-on to P2
			- Lifecycle workflows
			- Advanced access reviews
			- Advanced entitlement management
			- Insights + Reporting using machine learning
	- To assign a license to a user they must have a usage location property set
- Active Directory vs. Entra ID
	- AD: OUs, GPOs, Kerberos, LDAP and NTLM, hierarchical
	- Entra ID: administrative units, SAML, WS-Federation, OAuth, flat directory, no hierarchy, global

#### Managing Tenants
- planning our organization
	- design the tenant
		- how are we implementing security?
	- populate identity resources
		- add users, groups, devices, hybrid identity
	- manage apps 
		- identify apps to be used from app gallery & on-premises
	- use, automate and monitor
		- monitor admins
		- access reviews
		- automate user lifecycles

#### Creating and Managing Users
- What are users
	- identity objects with sets of permissions
	- permissions determine whether its external or internal
	- members of tenant - may or may not have administrator roles
	- external guests
	- identities are composed of JSON and easy to modify
	- users can have role assignments
	- users can own objects
		- e.g., user can be owner of a group object used to manage other users
			- manager manages permissions on other employees
- Types of users
	- Administrators
		- admin role assigned
	- Members
		- regular, default permissions, not external
	- Guests
		- External users invited into the tenant from outside
			- e.g., a contractor

#### Creating and Managing Groups
- Describe groups
	- provide same permissions, role assignments, licenses across many users
	- group owners - manager of the group
	- types
		- Security
			- used to manage access to shared resources for a group of users
		- Microsoft 365
			- used to give members access to a shared mailbox, calendar, files, etc.
	- membership types
		- directly (statically) assigned
		- dynamically assigned
			- assigns object as member of group based on properties of the object
				- e.g., new HR department user, dynamically assign to HR group
			- can't create them without at least a P1 license
		- dynamic device 
			- membership rules are created that automate group membership via device attributes

#### Creating Administrative Units
- What are administrative units
	- create a logical container inside the flat structure of Entra ID to restrict permissions to a subset (i.e., countries, departments, etc)
	- scope a role to just a specific administrative unit rather than the whole AD tenant

#### Self-Service Password Reset (SSPR)
- What is SSPR
	- users do it themselves, admins don't have to bother, reduce administative overhead
- The process:
	1. Localization (check browser locale settings)
	2. Verification of identity using various auth methods
	3. Authentication proces takes place after verification
	4. Perform the password reset
	5. Set whether IT is notified
		1. by default, ot her administators will be notified if an administrator resets their password
- Authentication methods
	- Mobile app (Microsoft Authenticator with app notification)
	- Mobile app code (time-based code that is cycled)
	- Email (authentication via an email external to Microsoft using codes sent to that email address
	- Mobile phone (SMS or phone call - not secure)
	- Office phone (authentication via non-mobile phone that prompts a user to press #)
	- Security questions (least recommended)
		- easily socially engineered
- SSPR considerations
	- enable and manage via groups and not on individual users
	- can't use security questions for admins, admins by default need MFA
	- administrators must use MFA
	- require P1 or P2 license, Microsoft 365 Business Standard or higher

#### Entra ID Device Management
- Device identity
	- device used by a user to authenticate
	- pull the device itself into Entra ID
	- register the device, control resources the device is accessing
- Registration options
	- Microsoft Entra ID Registered
		- least restrictive
		- allows BYOD with a personal Microsoft or local Windows account
		- Windows 10, iOS, iPadOS, Android, macOS
	- Microsoft Entra ID Joined
		- device is owned by org and accesses Azure AD through work account
		- device identities only exist in the cloud
		- InTune
		- Windows 10 and Server 2019
	- Microsoft Entra Hybrid Joined
		- like the above but identity also exists in an on-premises Active Directory domain
		- Windows 7, 8.1, 10, Server 2008 and later


























































































