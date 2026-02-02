Describing Azure Monitor
- end-to-end monitoring for applications and infrastructure
- captures metrics and logs on monitored resources
- data sources can come from Azure resources or on-premises resources, also application code, OS-level logs and metrics, PaaS

Metrics vs Logs
- Metrics
	- short, time-based data
	- Frequently updated
	- near real-time
	- alerts based on numeric values
	- visualization via Metrics Explorer
- Logs
	- long, event-based data
	- sporadically updated
	- free-form and/or structured
	- stored in Log Analytics workspace
	- built-in Kusto query language to read and query logs

Monitoring Explained
- metrics and logs created from many different data sources
	- i.e., VMs, PaaS resources, OS, application code, etc 
- metrics can be viewed in a number of different places
	- i.e., Azure Monitor, Log Analytics, Storage Accounts, Event Hub
	- Azure Monitor -> Metrics Explorer
	- query Log Analytics with Kusto Query Language
- logs can be viewed in Storage Accounts and Event Hub
	- log data -> Event Hub -> Non-Azure destinations

Interpreting Metrics
- Metrics Explorer: Monitoring -> Metrics in left blade of any resource or search Monitoring in top search bar
- logs in left blade for any resource or in Monitor
- enable log analytics for specific resources in left blade Monitoring -> Diagnostic and choose what to allow diagnostics for