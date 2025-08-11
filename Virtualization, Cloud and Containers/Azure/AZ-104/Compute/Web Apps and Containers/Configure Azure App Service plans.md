### Implement Azure App Service plans
- plan defines a set of compute resources for a web application to run on
- Things to know:
	- underlying resources will be in the region of the App Service plan
	- App Service plan defines three settings:
		- 1. Region
		- 2. Number of underlying VM instances
		- 3. Size of underlying VM instances (small, medium, large)
	- can continue to add more applications to the same plan, as long as the plan has enough resources to run all the apps

#### How applications run and scale in App Service plans
- plan is the **scale unit** of App Service apps, e.g., if plan has 5 underlying VMs, then all apps run on all 5 VM instances; all apps in the plan will autoscale if autoscaling is enabled and apps are scaled out and in together
- Scaling in App Service plan pricing tiers:
	- **Free and Shared tier** - apps share CPU minutes on a shared VM instance; no scaling out
	- **Basic, Standard, Premium & Isolated**:
		- apps run on all VM instances in the plan
		- all deployment slots run on the same VM instances
		- diagnostic logs, backups, WebJobs all use CPU cycles and memory on the same VM instances

#### Things to consider
- save money by putting multiple apps on the same plan
- be careful about plan capacity - determine app resource requirements and ensure the plan has enough capacity
- overloading a plan could cause downtime for all apps on the plan, new and existing
- isolate apps in a new plan when:
	- app is resource-intensive
	- needs to scale independently
	- needs to be in a different geography

------
### Determine Azure App Service plan pricing
- Six categories of pricing tiers for plans
- See [chart](https://learn.microsoft.com/en-us/training/modules/configure-app-service-plans/3-determine-plan-pricing)
- Tiers:
	- **Free and Shared** - run on same instances as apps for other customers; dev/test; no SLA
	- **Basic** - low traffic requirements; pricing based on size and number of instances; built-in load balancing; Basic with Linux runtime supports Web App for Containers
	- **Standard** - production; pricing based on size and number of instances; built-in load balancing; autoscaling; Standards with Linux runtime supports Web App for Containers
	- **Premium** - enhanced performance for production; upgraded Premium v2 uses Dv2-series VM instances; SSD storage; double memory-to-core ratio; autoscaling
	- **Isolated** - mission critical workloads that need to be in a vnet; run apps in a private, dedicated network; Dv2-series VM instances; SSD storage; double memory-to-core ratio; private network environment running instances is called **App Service Environment**; scale to 100 instances with more available upon request
	
----
### Scale up and scale out Azure App Service
- Scaling out increases instances; scaling up adds more CPU, memory and storage - scaling up requires increasing the pricing tier
- Things to consider:
	- Start low and scale up plan as needed
	- autoscaling can reduce costs and help end users
	- scaling doesn't require redeployment
	- databases and storage accounts can scale independently, accessed through private or service endpoints

-----
### Configure Azure App Service autoscale
- specify min, max instances in a set of rules and conditions
- vm instances automatically adjusted based on rules set
- autoscale setting is read by autoscale engine; autoscale settings grouped into profiles
- autoscale rules include trigger and scale action (in or out); trigger can be metric or time based
- autoscale engine can use notifications - when an autoscale event is triggered, an email can be notified or it can make a call to one or more webhooks
- things to consider:
	- make sure to set min and max instance counts - more economical
	- set a safe default instance count
	- always scale in to reduce costs when metrics decrease or outside peak time
	- always use autoscale notifications - important to be aware of how the application is performing as the load changes