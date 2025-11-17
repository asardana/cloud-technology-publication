Simple Storage Service
There are 2 types of storage-

Object Based – Used for storing files like photographs, videos, documents etc.
Block Based – Used for database storage, installing operating system etc.
You can read more about cloud storage in my post on Cloud Storage Types – Object, Block and File

AWS Simple Storage Services (S3) is an object storage primarily used to store files on the cloud. File size can be from 0 bytes up to 5 TB. Files are stored in buckets. Bucket names in S3 are global i.e. the bucket name is unique across all the regions.

S3 is a key value storage with each object having –

Key – Name of the object used for partitioning the object storage. It’s important to have random keys to distribute objects across different partitions. Salting techniques can be used to add randomness to the keys.
Value – Data made up of sequence of bytes
Version Id – Used for versioning the objects
Metadata – Additional Data about the object being stored
Subresources
Access Control List (ACL)
Torrent
Data Consistency Model-

Read after Write consistency for new objects – Reading new objects just after creating them in S3 would always be consistent with what was written into S3.

Eventual Consistency for updating or deleting the objects – Reading objects just after updating or deleting them in S3 may or may not be  consistent with what was updated into S3.

Few important AWS S3 features-

Life cycle management
Can be used along with versioning on both the current and old versions
Object size must be 128 KB
Typical workflow
S3 Object Creation (S3 Standard) –> S3 IA (min 30 days after creation) –> Glacier (min 60 days after object creation) –> Delete
Tiered storage
S3 – 99.99% Availability and eleven 9’s durability
S3 Infrequently Accessed – Less frequently accessed data and cheaper than S3 offering 99.9% Availability and eleven 9’s durability
Reduced redundancy Storage – 99.99% Availability and 99.99% durability
Glacier – Least expensive for archiving the data, however could take some time to retrieve the data from archive.
Built for 99.99% availability and eleven 9’s of durability
Versioning
S3 allows retaining old versions of the object. When an object is deleted, a delete marker is added to make the object invisible in the bucket.
S3 objects stored in the buckets without versioning enabled has ‘null’ version id. When the object is updated using the PUT, POST, COPY command, it replaces the existing object with ‘null’ version id.
If the existing object is updated after the versioning is enabled, old copy of the object will still have ‘null’ version id, however the updated object will be stored in the bucket with its own unique version number.
If the object is updated after suspending the versioning, S3 will store the object with ‘null’ version id and replace the existing copy of the object in the bucket which has ‘null’ version id (if object was originally created with ‘null’ version id).
Encryption
In Transit – Provided by SSL/TLS
At rest
Server Side Encryption
SSE-S3 (S3 managed Keys)
SSE-KMS (Key Management Service managed Keys)
SSE-C (Customer Provided Keys)
Customer Side Encryption
AWS will encrypt the object at rest with Server Side Encryption based on the value of x-amz-server-side-encryption header in the REST API request. AWS supports block cipher AES 256.
Bucket Names
It is recommended that the bucket names comply with DNS naming standards
Bucket names can contain lowercase letters, numbers, and hyphens with each label separated by a hyphen
Must start and end with lowercase letter or number
Bucket names cannot be formatted as IP addresses
Must be at least 3 characters and at the most 63 characters long
Multi-part upload
S3 support multi-part upload for large objects. AWS recommends to use multi-part upload for objects larger than 100 MB and is required for objects larger than 5 GB.
After all the parts of the object are uploaded, user will need to make a call to CompleteMultipartUpload operation to re-assemble the object
Secure data using ACL and bucket policies
Bucket Policies apply to all the objects stored within the bucket
ACL can be applied to individual object within the bucket
Transfer Acceleration – S3 offers transfer acceleration by leveraging the Cloud Front distribution that has superior networking between the S3 infrastructure and Cloud Front distributed edge locations.
Cross Origin Resource Sharing (CORS)
CORS allows the web application loaded in one domain to access resources in another domain. S3 supports CORS whereby java script in one S3 bucket can access the resources stored on another bucket. Bucket permissions allow the CORS configuration to be added for allowing the access from the client domain.
Cross Region Replication
S3 allows objects in one region to be replicated to buckets in another region
Versioning should be enabled on both the source and destination bucket
Replication can be enabled between 2 buckets only i.e. one to one bucket replication
Bucket ownership is not transferable
Storage Gateway
AWS storage gateway service enables hybrid storage between on premises data center and the AWS cloud.This service allows the data to be securely stored in the cloud and be accessible to the applications running locally in the data center.

Types of Storage Gateways-

File Gateway – Files stored in S3 buckets are accessible thought the NFS mount point.
Volume Gateway – Data written to the disk volumes using the iSCSI block protocol can be backed up as point in time snapshots and stored in AWS as EBS snapshots.
Stored Volume – All the data is stored locally on the mounted storage volumes and backed up asynchronously to AWS S3 bucket in the form of AWS block storage.
Cached Volume – All the data is stored  in AWS S3 while retaining the frequently accessed data locally in storage gateway.
Tape Gateway – Durable and cost-effective solution to archive the data in the AWS cloud. Allows the storage of the data in virtual tape cartridges.
