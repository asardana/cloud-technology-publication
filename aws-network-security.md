One of my earlier post on AWS Virtual Private Cloud described  the basics of VPC including some of the security features it offers to control which packets move in and out of the VPC. In this article let’s look at the VPC network security in further detail.

Following diagram shows an example of how the security groups and ACLs are associated with the subnets defined within a custom VPC.

VPCSecurity.png

Security Groups
Security groups control the inbound and outbound network traffic at the instance level. It’s the first layer of defense. One or more security groups (max 5) can be assigned to an EC2 instance. Each instance in a subnet can be assigned to the same Security Group or different Security Groups. Default security group is automatically assigned to the EC2 instance if no security instance is selected at the time of launching the EC2 instance.

Security Group supports ‘allow’ rule only which are stateful i.e. if an inbound rule is defined to allow the traffic then the outbound traffic for that connection is automatically allowed and vice versa for the outbound rule. All the rules are evaluated before deciding whether to allow the traffic or not. Any change in the Security Group rule is applied immediately. Security groups cannot be used for blocking specific IP addresses.

Default Security Group – Each VPC comes with a default security group which allows the instances in the default security group to talk to each other.
Inbound – Allows inbound traffic from instances assigned to the same security group
Outbound – Allows all outbound traffic
Custom Security Group – Custom security groups can be defined which does not allow the instances in the security group to talk to each other.
Inbound – When a new custom security group is created no inbound rules are defined and hence all the inbound traffic is automatically denied
Outbound – Includes an outbound rule that allows all the traffic
Access Control List
ACLs control the inbound and outbound network traffic at the subnet level. It’s the second layer of defense.  ACL rules are applied to all the EC2 instances created within the associated subnet. Each subnet must be associated with only one ACL. If a custom ACL is not associated with a subnet explicitly, the default ACL is automatically assigned to the subnet. One ACL can be associated with multiple subnets. ACL can be used for blocking specific IP addresses.

It supports both the allow and deny rules which are stateless i.e. rules for the return traffic should be explicitly defined. The rules are evaluated in the order in which they are defined (ascending rule number) to decide whether to allow the traffic or not as soon as the first rule matches.

Default ACL – It allows both the inbound and outbound network traffic
Custom ACL – It denies all the inbound and outbound traffic unless rules are added to allow the network traffic. Ephemeral ports need to be explicitly defined in both the inbound and outbound rules when creating the custom ACL. When a host initiates a network connection, a unique ephemeral port is automatically assigned as per the TCP/IP protocol when the packets are sent to the destination. The ephemeral port is used to route the return packets back to the host process which is associated with the ephemeral port.
Flow Logs
Flow logs allows to capture the information about the IP traffic going to and from the EC2 instances created in the VPC. Flow logs data is published to the CloudWatch logs which can then be used to troubleshoot any issue with the IP networking.
