A load balancer is a device that acts as a reverse proxy and distributes the application traffic across multiple servers. This results in increased capacity and greater reliability of the applications running behind the load balancer.

Generally load balancers are grouped into 2 types

Layer 4 load balancer – Acts on the data available in network and transport layer such as TCP, UDP, FTP etc.
Layer 7 load balancer – Acts on the data available in the application layer such as HTTP
AWS Elastic Load Balancer (ELB) automatically distributes the incoming application traffic across multiple applications, microservices and containers hosted on Amazon EC2 instances in multiple Availability Zones.

ELB is configured to accept incoming traffic on one or more listeners that checks for the client connection requests. It is configured with a protocol and port number for the connections with the client and sends the traffic to EC2 instances registered with the ELB.

When an Availability Zones is enabled on the load balancer, ELB creates a load balancer node in the Availability Zone. EC2 instances in an Availability Zone which has not been enabled on the ELB won’t receive the traffic.

ELB Features
Elasticity – ELB is inherently elastic and automatically scales to meet the increased incoming traffic load.
Security – ELB works with VPC such that security groups could be assigned to restrict which ports are open to a list of allowed sources. It also integrates with the AWS Certificate Manager to make it easy to enable the SSL connection for the application.
Integrated – ELB is integrated with other AWS services like CloudWatch, Route53, Auto Scaling etc. that makes it easy to design and implement robust solutions.
Highly Available – ELB automatically route traffic to multiple EC2 instances running across multiple availability zones. ELB itself runs in multiple availability zones. It also monitors the health of the EC2 instances and takes any unhealthy instances out of rotation.
Choice of Classic Load Balancer (Layer 4) vs Application Load Balancer (Layer 7)
Classic Load Balancer – Layer 4
Supports both the TCP and SSL protocols
Incoming client connection is bound to the server connection
Does not allow any header modification
Proxy protocol prepends the source and destination IP address along with the ports to the request
Application Load Balancer – Layer 7
Supports both HTTP and HTTPS protocols
Incoming client connection is terminated at the load balancer and pooled to the servers behind the load balancer
Headers may be modified. For instance, X-Forwarded-For header is added that contains the client IP address
Supports path based routing which allows the requests to be routed to different applications running behind a single load balancer
Supports containers
Following diagram shows how the ELB configured in multiple availability zones can be used to load balance the EC2 instances which are also running in multiple availability zones. This type of network topology provides high availability and fault tolerance by leveraging redundancy across multiple availability zones.

elb-example

ELBs can be configured for cross zone connection such that the ELB in availability zone 1 can send the application traffic to EC2 instances running in availability zone 2 and vice versa. If cross zone load balancing is disabled, traffic is evenly distributed across the ‘Availability Zones’ irrespective of the number of instances running in each Availability Zones. This could lead to uneven distribution of the traffic at the instance level if there are more instances running in one Availability Zone compared to the other Availability Zone. If cross zone load balancing is enabled, traffic is evenly distributed across all the ‘EC2 instances’ running in enabled Availability Zones.

Cross zone load balancing is always enabled for an Application Load Balancer and is disabled by default for a Classic Load Balancer.
