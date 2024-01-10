Types of data--:

# structured data

- Also called **relational data**
- Data that adheres to a schema.
    - Defines table, fields, clear relationship between two
- Can be stored in e.g. database table with rows and columns.
- AZURE SERVICES 
  * AZURE SQL Database
  * Azure Cosmo DB*
# semi strutured data

- Also called as **non-relational** or **NoSQL data**.
- Doesn't fit neatly into tables, rows, and columns.
- Instead uses tags or keys that organize and provide a hierarchy for the data.
- Azure services:
    - Azure Cosmos DB (MongoDB API, Cassandra API)
    - Azure Table Storage
    - Azure Queue Storage

egs--> books blogs html etc
# unstructured data
it has no designated schema ,it consists of differnt type and differnt data having diffrnt properties
 egs--> pdfs jpgs mp4 etc
- Azure services:
    - Azure Blob Storage
    - Azure File Storage
    - Azure Data Lake Storage
    - Azure Disk Storage


FOR starting with azure storage account 
we have to take in consideration that  wht type should be teken such that as

the type of account determines that there r several redundancy options --:
# redundancy in primary region

* LOCALLY REDUNDANT STORAGE --LRS

it replicates ur data 3 time over the single date center in primary region.
it provide durability of 11 nines for given year
if a natural disaster happens then may be all the data will be destroyed

![[Pasted image 20231103131857.png]]


* Zone REDUNDANT STORAGE --GRS

it replicates ur data 3 times over the 3 differnt availability zone, one data replicated for each zone
it gives durability of 12 nines for given year
if a zone becomes unavailable  the azure repoints to other zone where the data is replicated
read and write is available in primary region
![[Pasted image 20231103131916.png]]



* READ ACCESSS GEO REDUNDANT STORAGE --RA-GRS
# redundancy in secondary region

* GEO REDUNDANT STORAGE --ZRS

-> it replicates the data 3 times synchronously within the physical location in primary region using LRS
-> after that it copies data synchronously to the physical location in secondary region which will be the paired region of that primary region.
-> it gives the durability of 16 nines

![[Pasted image 20231103131950.png]]


* GEO ZONE REDUNDANT STORAGE --GZRS

->  it cones with high availability provided by redundancy across availablity zone with protectionfrom regional outages
-> the data is replicated in 3 availability zone in the **primary region** which is a **Zone redundant storage** such that it consists of 3 availability zone and the data is replicated in each zone one time 
-> and then in secondary region it has local redundant storage in which the data is replicated 3 times in single datcentr
-> it gives the durability of 16 nines

![[Pasted image 20231103132010.png]]



* READ ACCESS GEO ZONE REDUNDANT STORAGE --RA-GZRS

the configuration of replication is ame as of the GZRS ; 
its just that the will be available only as read access



# storage services in azure

The Azure Storage platform includes the following data services:

- **Azure Blobs**: A massively scalable object store for text and binary data. Also includes support for big data analytics through Data Lake Storage Gen2.
- **Azure Files**: Managed file shares for cloud or on-premises deployments.
- **Azure Queues**: A messaging store for reliable messaging between application components.
- **Azure Disks**: Block-level storage volumes for Azure VMs.
- **Azure Tables:** NoSQL table option for structured, non-relational data.



# Benefits of Azure Storage:-

* ### durable and highly available
* ### secure
* ### scalable
* ### accessible
* ### managed


## Azure Storage data services

The Azure Storage platform includes the following data services:

* Azure Blobs:
	A massively scalable object store for text and binary data. Also includes support for big data analytics through Data Lake Storage Gen2.
* Azure Files: 
	Managed file shares for cloud or on-premises deployments.
* Azure Elastic SAN (preview): 
	A fully integrated solution that simplifies deploying, scaling, managing, and configuring a SAN in Azure.
* Azure Queues:
	A messaging store for reliable messaging between application components.
* Azure Tables: 
	A NoSQL store for schemaless storage of structured data.
* Azure managed Disks: 
	Block-level storage volumes for Azure VMs..
## AZURE BLOBS STORAGE

* also known as azure blobs
* good foe very large objects like video files or bitmaps
* unstructured data can be stored , i.e there is no restriction on what of kind of data should be stored.
* it can mange 1000 of simultanous uploads and , masssive amounts of video data , constant increse of log files, and is available aalll over the internet
* it lets u stream large videos or audios
* send uh large video file 
* store data for backup and disaster recovery and archiving
* we can store 8TB of data for vm in blobs

### storage tiers
* #### Hot storage 
  it store that type of data which is required frequently
* #### cool storage
  optimizes the data which is not to be accessed or required for atleast 30 days
* #### archive storage
  for the data that r rarely accessed and stored for at least 180 days with flexible latency requirement.

### 3 types of blob storage
* #### Block blob
  they r composed of blocks and r ideal for storing text and binary fikes and for uploading large files efficiently.
* #### Append blobs
  they r optimized for  append operation, making them ideal for logging scenarios 
  * #### page blobs
    they r made of 512 byte pages , which provide the ability to read/write  arbitrary range of bytes. 
    thse r ideal for storing index based and sparse data structure like OS and data disk for VMs
	they only use the hot tier


### AZURE DISK STORAGE

* also known as azure disk
* provides disk for VM , appliction, and ther devices to access and use as they need
* allows data to persistently stored and accessed from an atteched virtual hard drive.
* it can be managed by azure or by user
* use case
  --> lift and shift
  * storing that data which is not required to be accessed from outside the virtual machine to which the disk is attached.
* different sizes and performance
  --> solid state drives SSD
  --> hard disk drives HDD


### azure file storage
* known as azure files
* file share that u can access and manage like a file server.
* fully managed file share in the cloud that are accessible via the industry standard server message block (SMB) protocol
* ensures the data is encrypted at rest and in transit
* can be mounted concurrently by cloud or on-premises windows, linux, and macos
* when we create file share on azure we get an option of file type and further we get a connection script which we have to run on powershell prompt after runnimg it successfully we can go to my computer and we cane see the file that we have created on the azure acn be visible on the local desktop and if create one file on that drive it can displayed on the azure portal as well.

### azure files is used to
* #### replace or supplement on-premises file server
* #### lift and shift
* #### simplify cloud deployment 
* #### containerization
### key benefits of azure file
* #### easy to use
* #### shared access
* #### fully managed
* #### scripting and tooling
* #### resiliency
* #### famailiar program
