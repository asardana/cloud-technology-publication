NAT Overview
Network Address Transaction (NAT) is a technique of assigning a public IP address to a host or a group of hosts within a private network such that all egress network packets have the same public source IP address. NAT helps in limiting the number of public IP addresses required for a private network to connect to the internet and also allows to hide the private IP address space from the outside world. All the hosts inside a private network could use the same public IP address to send the packets to the internet. The destination will receive the packets from different hosts that looks like coming from the same host as all the packets will have the same source public IP address. A NAT table is needed on the router or any NAT enabled device for the ingress packets to be routed back to the respective hosts in the private network.

Following diagram shows an overview of how the NAT enabled router translates the private IP address of all the devices inside the private network to the public IP address of the router before the packets get routed to the internet.

nat

By default a private subnet created in AWS VPC does not have an access to the internet. NAT can be used to allow the instances created in the private subnet to securely access the internet. It is possible to either set-up NAT instances or NAT gateways to allow the instances within the private subnet in AWS VPC to connect to internet. All the traffic from the private subnet is NATed via NAT instances or NAT gateways  configured in the public subnet.

NAT  Instances
NAT instances are EC2 instances running in public subnet that allows EC2 instances running in the private subnet to connect to internet. NAT instances requires a security group to be configured to allow the ingress and egress traffic from and to the internet. EC2 instance perform source/destination check by default to make sure that it is the source or destination of any traffic received or sent by EC2 instance. NAT instance should be able to send and receive traffic even if it’s not the source or destination of the traffic. In order for the traffic to pass through the NAT instances, you’d need to disable the source/destination check on the EC2 instance created for the NAT. Also, the main route table connected to private subnet would need to updated to allow the packets with destination to the internet (0.0.0.0/0) be routed to the internet via the NAT instances.

NAT instances can be made highly available by creating multiple instances and adding them to the Elastic Load Balancer group. The network bandwidth depends upon the bandwidth of the EC2 instance type.

NAT Gateway
NAT Gateway is preferred over NAT instances and more commonly used since it’s fully managed by AWS and offers several benefits. NAT gateway is created in a public subnet and assigned an elastic IP at the time of creation. Once the NAT gateway is created, the main route table attached to the private subnet can be updated to allow the packets with destination to the internet (0.0.0.0/0) be routed to the internet via the NAT gateway. NAT gateway doesn’t need to be behind a security group as opposed to NAT instances which are always behind a security group.

NAT gateway are highly available and created with redundancy in the availability zone. NAT gateway supports the bandwidth bursts of up to 10 Gbps.
