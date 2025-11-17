DynamoDB is a NoSQL database service that provides consistent, fast, scalable and reliable access to the fully managed database in the cloud. It supports both the key-value and document data models widely used in e-commerce, IoT, mobile and any internet scale application. DynamoDB stores the data on SSD storage and replicates it across 3 availability zones. It can be scaled out to meet the increased throughput demands without any downtime.

Data Models
Key-Value database allows to store the values against the indexed keys allowing the access to the values only by the keys. The values stored are transparent to the database since it allows querying against the keys only. This is suitable for applications that want a very fast read and write access to the values stored against the keys i.e. non volatile cache.
Document database allows the values to be stored in a structured format i.e. document having key value pair attributes which collectively represent an item in the table (a.k.a. document). It allows creation of secondary indexes and thus allowing the querying of the table against attributes other than the primary key. This data model is more suitable for e-commerce applications storing millions of products along with different attributes and to be able to query the database on multiple product attributes.
Consistency Models
Eventual consistency reads – Provides the best read performance with no guarantee that the data read after the write is the most recent data since it might not be consistent across the multiple nodes. Consistency across all the copies of the data is usually achieved within one second.
Strong consistency reads – Guarantees that the read performed returns the result which is consistent across all the copies of the data. Any write which is not consistent across the nodes won’t be visible in the result.
Primary Keys
Single attribute key – Key is made up of single attribute known as partition key. The key is hashed to determine the partition where the item is stored.
Composite key – Composite key is made up on the partition/hash key along with the sort key. It’s used in the scenario where multiple items have same partition key sorted based on the sort key. Hashed partition key determines the partition and sort key sorts the items within the partition to have a unique primary key for the items.
Key lengths
Partition/Hash Key – Minimum 1 byte and maximum 2048 bytes
Sort Key – Minimum 1 byte and maximum 1024 bytes
Indexes
DynamoDB allows to create one or more secondary indexes on the table for querying the table on non primary key attributes. It allows up to 5 Local Secondary Index and up to 5 Global Secondary Index.

Local Secondary Index (LSI) – Has the same partition key as the base table with different sort key. LSI can only be created when the base table is created and cannot be removed or modified after base table creation. LSI partition scope is limited to the base table partition having the same partition keys. For each partition key value, the total size of all indexed items is limited to 10 GB or less.
Global Secondary Index (GSI) – Has different partition key and sort key than those on the base table. GSI can be created at the time of table creation or can be added later as well. Query on the GSI can span on the base table across all the partitions. There are no size restrictions for GSI.
Streams
DynamoDB Streams is used to capture any modifications made to the tables. It stores the data for 24 hours.

Item added –  Stream captures all the attributes of the item after its created.
Item updated – Stream captures all the updated attributes of the item  before and after its updated.
Item deleted – Stream captured all the attributes of the item after its deleted.
Query, Scan and BatchGetItem
Query is used for searching the item based on the primary key. Partition key is mandatory to search for an item or list of items. Sort key is optional and only required to filter the result set. Result set is always sorted by the sort key with the default sort order of ascending. The sort order can be reversed by setting the ScanIndexForward parameter to false. Query is eventual consistent by default and can be changed to strongly consistent read.

Scan returns every item in the table. Items can be filtered, however the filter is only applied after scanning the whole table. Hence query is more efficient than scan.

For both the query and scan, the default is to return all the attributes of an item, however ProjectionExpression can be used to limit the attributes that are returned in the result set.

Scan operation performs eventually consistent reads by default and can return up to 1 MB of data. Following techniques can be used to minimize the impact of scan operation on table’s provisioned throughput-

Reduce Page Size – The number of records returned in a single scan operation can be controlled by setting the page size in the request. Scan operation provides a Limit parameter that can be used to control the number of read operations consumed for scanning the items.
Isolate Scan Operations – Application can create a shadow or a duplicate table on which the scan operation can be executed.
BatchGetItem operation retrieves the attributes of one or more items from one or more tables based on the primary key. Single operation can retrieve up to 16 MB of data and can contain up to 100 items in the response.

Provisioned Throughput
Provisioned throughput capacity is reserved for reads and writes when the table is created. This ensures that the resources are reserved to meet the throughput needs of the application. Tables with secondary index consumes additional capacity units. If the read or write exceeds the provisioned throughput, DynamoDB may throttle the request with HTTP 400 error and ProvisionedThroughputExceededException.

Read capacity unit depend on the item size and whether the read is eventual consistent or strongly consistent. Write capacity unit depends on the item size only.

Read capacity unit
Rounded to the multiples of 4 KB
Eventual consistent reads (default) – 2 reads/second
Strongly consistent reads – 1 read/second
Write capacity unit
Rounded to the multiples of 1 KB
All writes – 1 write/second
Total item size including the attribute name and attribute value cannot exceed 400 KB.

Optimistic Locking and Conditional Writes
DynamoDB supports optimistic locking and conditional writes to avoid the items or data being overridden in a multi user environment.

With optimistic locking, each item in the table has a version number attribute that is validated whenever the item is updated by a user. The version number of the item during the update should match the version number when the item was read from the table. If the version number doesn’t match then the update operation fails since the item got updated by some other user.

With conditional write, an operation succeeds only when the item attribute meets one or more expected conditions. Conditional writes are idempotent i.e. the conditional writes can be executed multiple times without any side effect. Conditions can be specified in the ConditionExpression parameter in PutItem, DeleteItem and UpdateItem requests.
