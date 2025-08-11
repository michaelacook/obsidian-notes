#### Azure Resource Manager
- resource groups - logical container for grouping resources
	- group based on anything (lifecycle, security)
	- contained within an azure subscription
- subscription
	- logical construct groups resource groups
	- billing unit - accumulated cost of all resources in rgs in subscriptions
	- create subscriptions for different purposes
- subscriptions interact with azure resource manager
	- top level resource in azure
	- orchestration layer
	- every way of interacting with azure uses resource manager via rest-api endpoints
	- sends requests to azure resource providers
	- portal/cli/cloud shell->ARM->Azure Resource Providers->perform action on resources
- can't use ARM to manage other people's resources
	- Azure AD/Entra ID - users interact with ARM
	- subscription can only have a trust relationship with single tenant at a time
	- tenant can have trust relationship with multiple subscriptions

#### Using ARM templates
- Resource Manager templates in JSON
- Deploying graphically in the portal uses ARM templates under the hood
- Infrastructure as Code (IaC)
- deploy at different scopes we choose
- automate infrastructure deployment/recover environment
- components of ARM template
	- parameters - dynamic runtime data passed into the template
	- variables - hard-coded variables
	- resources - specific resources to deploy, use variables or parameters
	- outputs - return info from the execution of the template. i.e, get back public IP
- can get pre-made templates from github, customize as needed
- export a template from a deployment:
	- at the Review and create page you can download the ARM template
	- can also see the template in the left pane under Template after the deployment is complete
- Deploy a custom template
	- use quick start templates

#### Bicep Files
- domain-specific language (DSL)
- has control structures like conditionals and loops
- Simpler alternative to using ARM templates for IaC
- Bicep and ARM use different schemas and a different declarative language
- JSON is more verbose and harder to read for humans - more emphasis on computer readability
- abstraction layer ontop of ARM templates - translated to an ARM template
- sections
	- parameters
	- variables
	- resources
- symbolic names in Bicep - not the name of the resource
	- used as a reference in case any other resource needs to reference the resource

```Bicep
resource webAppPortal 'Microsoft.Web/sites@2022-03-01' = {
	name: "MyWebApp"
	...
}
```

- `webAppPortal` is the symbolic name that other resources in the file can reference if needed 
- Can run a deployment with a bicep file in PowerShell:

```powershell
New-AzResourceGroupDeployment `
	-Name NameOfDeployment `
	-ResourceGroupName GroupName `
	-TemplateFile \path\to\file.bicep
```

- And in Bash:

```bash
az deployment group create --name name \
	--resource-group resourceGroup \
	--template-file file.bicep
```

- if you want to be prompted to input a value for a variable each time you run the deployment, specify the variable but don't assign it a value

##### Migrating ARM templates to Bicep files
- To manually build an ARM template from a Bicep file, run:
	 `az bicep build --file file.bicep --outfile file.json`
- To manually decompile an ARM template to a Bicep file run:
	 `az bicep decompile --file file.json`
- This is a best-effort process. The decompilation may have unexpected results, always check