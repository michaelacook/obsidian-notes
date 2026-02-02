### Understanding Storage Accounts
- top level resource for storage purposes
- underneath, multiple subservices that has its own purposes
	- i.e., Queue, Azure Files, Azure Tables, etc
- but all rely on the storage account
- storage name: unique globally, 3-24 characters, letters and numbers only

#### Components of storage accounts
- Account type determines features and costs
- Performance tier
- Replication - determines infrastructure redundancy
- Access tier - determines access levels (hot, cold, archive)

#### Storage redundancy
- options
	- **locally redundant** - redundant inside one data center
	- **zone redundant** - redundant across two AZs
	- **georedundant** - redundant across one AZ in one region, and another region
	- **geo-zone redundant** - redundant across two AZs across two regions
- always have at least three copies of the data, but how are they spread out?
- can have up to 6 copies of data


### Conceptualizing Azure Blob Storage

#### Describe Azure Blob storage
- subservice of a storage account
- object-based and easily accessible from HTTP/REST
- similar to S3 in AWS
- image and video, text, log files, VHDs
- everything is stored in containers (same as bucket in AWS)

#### Components of Blob Architecture
- Blob Service under a storage account
	- Blob Container - logical container for storage of objects/blobs
- Types of Blobs
	- **Block Blobs** - Storing images or videos. Best suited for streaming
	- **Append Blobs** - log files
	- **Page Blobs** - Virtual machine disks

##### Container Access Levels
- By default, public access to blobs is granted at the storage account level
- access levels 
	- private (no anonymous access)
	- blob (anonymous access to blobs but not the container itself)
	- container (anonymous access to container and blobs it contains)


### Configuring Blob Object Replication

#### Describe Object Replication
- feature of Azure storage to asynchronously copy block blobs between storage accounts
- source and destination storage account
- both need to have versioning and `$blobchangefeed` enabled
	- do this under Data protection when creating a storage account
	- don't need to enable `$blobchangefeed` on destination storage (unless you have bidirectional replication?)
- in the same region, across regions, subscriptions and tenants

#### Conceptualizing Object Replication
- two storage accounts in two different regions, and users in those regions each need access to the same block blogs, so replicate them between storage accounts in different regions to cut down on latency
- Benefits of Object Replication
	- Minimized latency
	- for read operations
- increased efficiency
	- processing block blobs in different regions
- data distribution
	- processing and analyzing data in one location that replicates to other regions
- cost optimization
	- moving replicated data to the archive tier can reduce costs


### Configuring Blob Lifecycle Management
- requires at least GPv2 storage account

#### Describe lifecycle management
- capability for blob storage
- automate the process of moving blobs through access tiers as they age
- might begin with hot access tier, and ultimately end with deletion
- needs for data changes as it ages, this can be automated
- optimize storage costs
- Configuring
	- Select Lifecycle management under Data management on the storage account
	- add a rule 


### Configuring Azure Files

#### Describe Azure Files
- sub-service of a storage account
- managed fileshare service
- SMB/NFS
- Windows, Linux and MacOS can connect
- Can connect to on-premises with Azure File Sync
- Not a flat file strucuture like blob storage
- Win32 and POSIX

#### Components of Azure Files
- File service
- File share - file structure we are connecting to locally
	- files and folders

##### Connectivity Options
- Insecure connectivity - REST, SMB 2.1, SMB 3.0
	- only use if internal to the network
- Secure
	- REST and SMB 3.0
- Security
	- data encrypted at rest by default and in transit over HTTPS and SMB

#### Azure File Sync
- separate Azure resource that must be deployed in the same region as the storage account

##### Describing Azure File Sync
- extend Azure Files into on-premises Windows file servers
- Increase on-premises file servers
- locally cache frequently accessed files
- Windows Server 2012 R2 and later
- SMB, NFS, FTPS
- Requires File Sync agent

##### Components of Azure File Sync
- **Storage Sync Service** - Azure resource to extend Azure file shares into on-premises
- **Cloud Endpoint** - file share utilized in File Sync
- **Registered Server** - trusted on-premises file servers
- **Sync Group** - cloud end point and server end point such as a data volume

##### File Sync Cloud Tiering
- locally cache frequently accessed files, less frequently accessed go to cloud

### Configuring Azure File Sync
- Set up a file share in a storage account
- Deploy Azure File Sync to the same region as the storage account for the file share
- In Windows Server, turn off IE Enhanced Security Configuration in the Server Manager>Local Server dashboard
- Install the Azure PowerShell module on the server with `Install-Module -Name Az`
- Create a file share on Windows Server
- Install & configure the Azure File Sync Agent on Windows Server
- Go to Storage Sync in Azure and view registered server
- Create a sync group in the Storage Sync Service
	- cloud endpoint is the Azure Files share
- [Azure File Sync Introduction](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-introduction)
- [Azure File Sync Deployment](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-deployment-guide?tabs=azure-portal%2Cproactive-portal)


### Storage Network Access

#### Storage Access Options
- **Public endpoint** - all services public by default
- **Restricted Access** - only on certain IP ranges or virtual networks - using a firewall
- **Private Endpoints** - allow private IP access for resources in an associated virtual network

##### Public Endpoints
- this is default for storage services
- `https://<accountname>.<subservice>.core.windows.net/<resourcename>`
- can use storage account firewall to restrict access

#### Providing Network Access
- Under storage account>Networking>Firewalls and virtual networks
	- Allow access from selected networks
- Private endpoints
	- private connectivity to storage accounts
- [Secure an Azure Storage Account](https://learn.microsoft.com/en-us/training/modules/secure-azure-storage-account/5-control-network-access)


## Securing Storage Accounts

#### Azure Storage Encryption
- all data at rest is using Storage Service Encryption (SSE)
- Can require that all data in transit goes over HTTPS, exclude HTTP

#### Azure Storage Authentication
- management layer 
	- high level resource
	- data layer
- access keys - unlimited access to management and data layers
- shared access signature tokens (SAS) - an access signature generated from access keys, limited access at account or service level
- azure AD authentication - RBAC access identities to provide authentication (instead of access keys)
- [Securing Azure Storage Accounts](https://learn.microsoft.com/en-us/training/modules/secure-azure-storage-account/)


### Azure Jobs
- Moving large amounts of data between on-premises data center and Azure storage
- to/from Blob service
- import only into Files server, cannot export
- provide a carrier to take drives to Azure
- use supported drives
	- SATA
	- HDD
	- SSD

#### Describe Azure Jobs
- prepare disks (WAImportExport)
- Create job
- Ship drives to Microsoft
- Check job status
- Receive back disks from Microsoft
- Check data to ensure done correctly
- For export:
	- create job
	- ship drives (WAImportExport)
	- check job status
	- receive and unlock the disks with our data

### Storage Utilities

Describe Storage Utilities
- storage explorer
	- graphical
	- Windows, Linux, macOS
- AzCopy
	- PowerShell module

































































