Elastic Compute Cloud (EC2)
EC2 is a web service which provides re-sizable computing resources that could scale out or scale down very easily based on the varying work load. At a very basic level it’s nothing but a virtual machine running as a guest on the host machine with the help of a hypervisor.

Typically it could take an enterprise few weeks or maybe sometimes few months to provision a new server and harden it before it could be used by application teams to run software. EC2 provides a self provisioning capabilities and it takes only a few mins to provision a new server from the list of different server types to choose from. Also, it has the added benefit of reduced cost due to pay as you go model without any long-term commitments.

AWS provides EC2 instances of different family types depending upon the whether the workload requires larger memory, higher IOPS, graphics, dense storage capability or compute optimized resources. There are various AMIs (Amazon Machine Images) available which is kind of a blueprint of the list of software needed to be run on the EC2 instance. You can create your own AMIs and make them public to share with other users within the same region. AMIs can be made available in other regions, however you’ll need to copy them over to other regions.

EC2 instances are available as on demand, reserved or spot with different pricing models. On demand charges hourly while reserved instances provides a discount for reserving the capacity for up to 3 years. Spot instances are available based on the available capacity in the AWS region and the prices vary based on the demand and the ongoing bid price. Spot instances are mainly used by companies with a flexibility to run heavy workloads off hours when the demand for computing is low and the termination of the processing when the spot price exceeds bid price is acceptable.

Multiple IP Addresses

EC2 instances running in a VPC can have multiple private IP addresses. IP addresses are associated with the network interface attached to the EC2 instance. Secondary IP addresses can be assigned to network interface. Network interface can be attached to either a running or stopped EC2 instance. Additional network interfaces can be detached from the EC2 instance and attached to another EC2 instance.

Similarly multiple public IP addresses can be assigned to an EC2 instance to host multiple public facing websites. When more than one network interface is attached to an EC2 instance, AWS does not assign the public IP addresses automatically. You’ll need to assign Elastic IP addresses from the VPC EIP pool to multiple network interfaces attached to the EC2 instance.

In Amazon Linux, ec2-net-utils package helps in configuring the operating system with the new secondary IP address. It configures the additional network interfaces on the running instance, refreshes secondary IP address during DHCP lease renewal, and updates the related routing rules.

Auto Scaling

Auto Scaling enables you to follow the demand curve for your applications closely, reducing the need to manually provision the excess EC2 compute capacity in advance. Auto scaling involves creation of Launch Configuration and Auto Scaling group. New EC2 instances can be added automatically to the Auto Scaling Group when the average utilization of the EC2 fleet is higher than the set condition.

Auto Scaling group uses a launch configuration template to launch new EC2 instances. Launch configuration has information on the type of instances to provision, AMI, security groups etc. similar to what is provided while launching EC2 instances. Only one launch configuration can be associated with an Auto Scaling group. However, multiple Auto scaling group can have the same launch configuration.

Auto Scaling Group can contain EC2 instances in one or more availability zones within the same region. Auto Scaling group cannot span multiple regions. Auto Scaling attempts to distribute the EC2 instances evenly across the availability zones enabled for the Auto Scaling Group. It does this by launching new instances in the Availability Zone with the fewest EC2 instances.

Auto Scaling Policy is defined at the time of creating the Auto Scaling group. Auto Scaling policy uses CloudWatch alarms to trigger auto-scaling activities and have Elastic Load Balancers distribute traffic to EC2 instances running within the Auto Scaling group.

EC2 Placement Groups

A placement group is a logical grouping of EC2 instances within the same Availability Zone. Placement group cannot span multiple Availability Zones. The name of the placement group should be unique within the AWS account. Placement group benefits applications that require low network latency and high network throughput running in a single availability zone on multiple EC2 instances. Existing EC2 instance can’t be moved to a placement group, instead a new EC2 instance can be launched in the placement group by  creating the custom AMI from the existing EC2 instance.

IAM Roles and EC2 Instances

Applications running on EC2 instance requires credentials to make connection to other resources like S3, DynamoDB etc. to store and retrieve the data. One of the options is to generate the credentials (Access Key Id and Secret Access Key)  and have them stored on the EC2 instances allowing applications to securely access the resources. This requires developers to manage the credentials and rotate them manually based on the organizational policy and compliance needs. This approach is not secure since the credentials are shared and stored on the EC2 instance manually.

Instead of having developers manage the manage the credentials, the preferred approach is to use the IAM Roles. A role is assigned at the time of launching the EC2 instance and it provides temporary permissions to the applications running on EC2 instances. This doesn’t require developers to store the credentials on the EC2 instances manually. Applications running on the EC2 instance gets role-assigned temporary credentials to sign the requests to access the resources securely.

A role can be assigned to multiple instances, however only one role can be assigned to an instance. You can add additional policies to a role assigned to running EC2 instances that takes effect immediately. If no role is selected while launching the EC2 instance, it can be attached later to the running EC2 instance. Also, the role can be removed or replaced on a running EC2 instance.

Elastic Block Storage (EBS)
EBS is a disk in the cloud that provides a consistent block storage and can be attached to EC2 instances. EBS offers a high availability and durability since its automatically replicated in the availability zone. Amazon EBS volumes can range from 1 GB to 16 TB in size and can be attached to just one EC2 instance in the same availability zone. Once the EBS volume is attached to the EC2 instance, the user cannot see the attached device on the EC2 instance until the EBS volume is mounted on the EC2 instance.

There are five different types of EBS volume currently available

General purpose SSD (Solid State Drive) offering up to 10,000 IOPS. It provides a balance of both price and performance.
Provisioned IOPS SSD offering more than 10,000 IOPS and up to 20,000 IOPS. Designed for I/O intensive applications like large relational databases or NoSQL databases.
Throughput Optimized HDD – Generally used in the scenarios were large amount of data is written in sequential order. Some of the example scenarios are big data, data warehouse and log processing.
Cold HDD – Low cost storage for infrequently accessed workloads. Could be used for storing files where the files need to be stored locally to the EC2 instance instead of S3.
Magnetic (Standard) offering a cheap block storage option
It is important to remember that out of the above 5 EBS volume types, throughput optimized HDD and Cold HDD cannot be used as root volume.

EBS root device volume for default AMI cannot be encrypted, however when a copy of the AMI is created EBS volume can be encrypted. The root volume is deleted by default when an EC2 instance backed by EBS volume is terminated. In order to attach the root volume to another EC2 instance as secondary volume, EC2 instance must be stopped before detaching the root volume.

You can take a snapshot of the EBS volume that gets stored on S3. Snapshots are point in time copies of the volume and are incremental i.e. only those blocks that got changed since the last snapshots are moved to S3. These snapshots can then be used to create the volumes in other Availability Zones and Regions. Snapshots can only be shared if they are not encrypted. Snapshots of encrypted volume are encrypted automatically. Volumes restored from encrypted snapshots are also encrypted automatically.

You can read more about cloud storage in my post on Cloud Storage Types – Object, Block and File

 Elastic Load Balancer (ELB)
ELB distributes the incoming traffic to multiple EC2 instances which could be running in the same availability zone or it could also be set-up to load balance the traffic across EC2 instances running in multiple availability zones within a region. It helps in providing a high availability and fault tolerance by removing the non-healthy EC2 instances from the pool ensuring that the traffic is routed only to the healthy instances.

There are 2 types of ELB-

Classic Load Balancer – It works at either layer 7 (application) or layer 3 (network) to route the traffic to EC2 instances.
Application Load Balancer – It works on application layer and routes traffic based on the content of the request.
Protocols supported by ELB – HTTP, HTTPS, TCP, SSL
