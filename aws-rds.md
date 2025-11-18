Amazon Relational Database Service (RDS) is a fully managed and cost efficient database service that makes it easy to provision, manage, and scale a relational database in the cloud. Amazon RDS provides an option to choose from the 6 available relational database engines –

Commercial
Oracle
Microsoft SQL Server
Open Source
MySQL
PostgreSQL
MariaDB
Cloud Native
Aurora
Benefits of using the RDS service.

No Infrastructure Management
Cost Effective – Pay as you go model
Instant Provisioning – Database up and running in few minutes
Ease of Scaling up or down  – Resize the database instance and storage to get an appropriate configuration of CPU and memory
Compatibility with existing applications
When compared to DynamoDB which offers push button scaling without any downtime, RDS requires a bigger instance types or read replicas to scale the database for larger workloads.

RDS Backups
Automated Backups

AWS RDS creates automated backups of the database instance during the backup window selected for the primary instance. If no back-up window is selected, a default window of 30 mins is automatically assigned to the DB instance. Backup is taken from the stand-by instance if the RDS service is running in multi-AZ. Automated backup allows you to recover the database to any point in time during the retention period. Automated backups are saved according to the retention period specified for the DB instance. During automated backups, storage I/O maybe suspended briefly resulting in  an elevated latency for few minutes.

The retention period can be specified between 1 and 35 days with default value being 1 day when the DB instance is created using Amazon CLI or RDS REST APIs, or 7 days if the DB instance is created using AWS console.  The automated backup can be disabled by setting the retention period to 0 days. There is no limit for the number of automated snapshots that can be taken for a given region.

Automated backup takes a full daily snapshot of the DB instances and also stores the transaction logs throughout the day. During the database recovery, it will select the appropriate full day snapshot captured during the retention period and will apply the transaction logs in order to recover the database to a point in time up to a second. DB restoration creates a new RDS instance with a different endpoint than the original RDS instance from which it was restored.

Automated backups are deleted when the database instance is deleted after which the backups cannot be recovered. Automated backup allows you to recover the database in the same region as the database

Manual Snapshots

Manual snapshots are user-initiated and hence do not get deleted when the DB instance is deleted. It enables you to recover the entire database from a single snapshot. Manual snapshot is useful for the longer retention period beyond the 35 days limit with the automated snapshots.

Manual snapshots can be used for cross-region backup by copying the manual snapshot to a different region and then restoring the database. There is a limit of 100 manual snapshots per region.

RDS Features
High Availability

RDS provides High Availability by allowing DB instances to be created in multiple availability zones. Multi-AZ allows to create an exact copy of the database in a different availability zone than the primary DB instance. Any data written to the primary DB instance is automatically replicated to the stand by instance in a different availability zone within the same region.

The replication is synchronous and hence the database can be failed over to the stand by instance automatically without any manual instance. During automated fail-over the stand by instance is promoted to be the primary instance. This is done by updating the DNS CNAME record of the DB endpoint of the master to the endpoint of the stand by instance. This DNS fail-over can take up to 2 minutes and is completely transparent to the application since the DNS endpoint used by the application does not change.

RDS_HA

Multi-AZ setup is for High Availability and Disaster Recovery only. It is not designed to be used for improving the performance and scaling the database for heavy workloads.

Read Replicas

One of the options to scale RDS Database with read heavy workload is to create read replicas. It allows you to have a read only copy of your primary database. The data is automatically replicated asynchronously from the primary database to the read replica copies across multiple regions. Read replica can be used for reading the data only and cannot be used for writing the data. All the data is written to the primary DB instance.

It is possible to create up to 5 copies of read replicas either from the primary DB instance or read replica itself. Each read replica will have its own DNS endpoint.

Read replica can only be used for scaling. It is not designed for High Availability and Disaster Recovery. However, the read replicas can be promoted to primary for faster recovery in case of a disaster.

Read replicas can be created in a different region and currently supported for MySQL and MariaDB. While creating read replica, you can select a DB instance class different from the instance class of the primary DB.

Data Protection

In Transit Encryption
Can enable SSL for all the inbound and outbound traffic from the database instance
At Rest Encryption
Supports AWS Key Management Service (KMS)
Two tiered key hierarchy
Data Key encrypts customer data
AWS KMS master-key encrypts data keys
Provides an option to encrypt the database at the time of provisioning
Can only be encrypted at the time of database creation and cannot be removed once the database is encrypted
Unencrypted snapshots can be turned into encrypted snapshots while copying the snapshots
Cannot copy encrypted snapshots or replicate encrypted DB across regions
Aurora – Cloud Native Relational Database 
Amazon Aurora is a MySQL and PostgreSQL compatible cloud native relational database to provide better performance at a lower cost when compared to other relational databases.

Relational databases were not designed for the cloud with multiple layers of functionality (SQL, transactions, caching and logging) in a single monolithic stack storing the data in the storage layer. Aurora DB has a distributed design based on the Service Oriented Architecture. Logging and storage layer has been separated from the monolith and made distributed, scale-out and multi-tenant. Storage layer is distributed across 3 Availability Zones and striped across hundreds of nodes.

Aurora Features-

Scaling
Storage Scaling – Start with 10 Gb and scales with 10 Gb increments up to 64 Tb
Compute Scaling – Up to 32vCPUs and 244 Gb of memory
High Availability
Storage – Maintains 6 copies of the data on the storage nodes (2 copies each in 3 Availability Zones). These 6 storage nodes are on a peer to peer gossip network and used quorum system for read/write.
Designed to transparently handle loss of 2 copies of data without affecting write availability and  3 copies of the data without affecting read availability
Read Replicas
Aurora Replicas – 15 replicas with automatic fail-over. You need to assign a priority tier which is used for deciding which replica is promoted to primary during fail-over
MySQL Replica – 5 replicas without automatic fail-over
Single DNS read replica endpoint is possible for Aurora
