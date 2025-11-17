AWS Virtual Private Cloud (VPC) is a web service that allows provisioning of a logically isolated infrastructure in the public cloud with its own IP address range, subnets, internet gateway, ACLs and route table configuration. It can be thought of as an isolated data center in AWS. VPC does all the heavy lifting and makes it very easy for the enterprises and start-ups to create virtual data centers hosted by cloud providers with all the features like security, IP addressing, subnetting and routing. Hybrid cloud  which is a mix of on-premise and hosted cloud infrastructure is becoming  increasingly common for big enterprises  to reduce the capital expenditure and increase agility.

It is not uncommon for enterprises to have multiple security zones that partitions the infrastructure and resources based on similar characteristics like public or internal facing and data protection. Inter zone communication is usually controlled and restricted by having a firewall for all the traffic that flows between the security zones. There could be a public facing subnet that hosts the web servers that talk to the internet and take the requests from the end-users. Backend systems like application servers and databases are hosted on a private facing subnet that cannot be accessed directly from the internet. All the traffic from web servers hosted on the lower trust zone flows through the firewall to the application servers hosted on the higher trust zone.

AWS VPC provides defense in depth by allowing to define security groups, network access control list and the route tables to restrict the traffic between the public facing subnet and private subnet.

VPN connection can be created between the corporate data center and VPC to set-up a hybrid cloud environment where VPC is an extension to the corporate data center.

AWS automatically creates a default VPC when an EC2 instance is provisioned. All subnets within a default VPC have a route to the internet. EC2 instances created in a default VPC will have a public and private IP address.

When a new VPC is created it automatically creates a main route table, default security group and a default network ACLs. It doesn’t create any subnets as those are expected to be explicitly defined  by the user. It also doesn’t automatically create an internet gateway.

VPC IP Addresses
Private networks created in a VPC can use IP addresses anywhere in the following range listed with the Classless Inter Domain Routing (CIDR) notation-

10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
AWS allows the creation of a new VPC if the IP address block sizes are between  /16 netmask and /28 netmask. It also allows the users to select the multi-tenancy for the new VPC. VPCs can be created on the shared hardware or the dedicated hardware based on the needs of the user and other factors like compliance or regulations.

It’s best to avoid using IP address range that might conflict and overlap with other networks to which the VPC might connect. VPC is created within a specific AWS region and could span across multiple availability zones within the region.

Following diagram shows a VPC with a private IP address range of 172.20.0.0/16 and having 3 availability zones having subnets with their own IP address range falling under the VPC IP address range.

VPC with  /16 block has total 64K IP addresses available that can be assigned across all the subnets in the region. Subnets with /24 block has total 256 IP addresses that can be assigned within the subnet, however 5 IP addresses are reserved by AWS due to which only 251 IP addresses are available in each subnet.

vpc

VPC Routing
Route table contains a set of rules that determines how the network packets are routed from source to destination. VPC comes with a default route table. Each subnet should be associated with only one route table. Multiple subnets can be associated with the same route table. Based on the destination, either the packets can be routed locally within the subnet or can be routed to internet gateway with 0.0.0.0/0 destination which is a CIDR notation for matching any destination IP address. The most specific rule that matches the route gets applied to the packet.

VPC Network Security
Since VPC can make a connection to the internet, it is important to have a mechanism to control which packets move in and out of the VPC. There are 2 ways in which the VPC network can be secured-

Access Control List (ACL) – ACLs are analogous to the stateless firewall and can be applied at the subnet level.There is separate set of rules for ingress and egress traffic.
Security Groups – These are stateful rules which are defined at the host level. It controls what packets are allowed when someone initiates a connection and automatically tracks the connection to allow the response traffic back to the source.
Click here to read more about the VPC Network security.

VPC features
One subnet is associated with one availability zone only. A subnet cannot span multiple availability zones. Security groups, ACLs and route tables can span multiple availability zones.
Instances can be launched either in private subnet or public subnet
Custom IP addresses can be assigned in each subnet
Route table can be configured to connect to internet gateway which determines whether the subnet is internet facing or private
VPC can have only one internet gateway attached to it
Allows to define the security groups which are stateful
Allows to define ACLs at the subnet level which are stateless
VPC Peering
Allows to connect one VPC with another VPC via a direct network route using private IP addresses. Peering is done is star configuration and transitive peering is not allowed.
