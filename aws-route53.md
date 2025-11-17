In one of earlier posts on DNS, we looked at the basic functionality provided by the DNS service and some of the important concepts related to the DNS protocol.

AWS Route 53 is a distributed managed service that provides both the public and private DNS lookup service with a very high availability and scalability. It makes it easy to manage the application traffic globally through a variety of routing policies that allows low latency and fault tolerant architectures.

Route 53 can be used to configure DNS health check such that requests are routed to healthy endpoints only.

DNS Setup
Register a domain name – Domain can be registered with Route 53 or any registrar offering the domain registration service.
Create hosted zone in Route 53 – Route 53 automatically creates a new hosted zone when a new domain is registered with it. If the domain is registered elsewhere, a new hosted zone needs to be created in order to use the Route 53 DNS service.
Create record sets in the hosted zone – This step involves creating the A record, CNAME record, Alias record etc. within the hosted zone. Alias record is unique to Route 53 which is similar to CNAME and allows to map the domain to other AWS services like ELB, S3 website, CloudFront etc. having dynamic IP addresses.
Connect domain name to the hosted zone – This is known as delegation which involves updating the name server with the domain registrar. If the domain is registered elsewhere, name servers are updated with the registrar to point to Route 53 hosted zone.
Routing Policies
Simple Routing Policy

Simple Routing Policy is the most basic routing policy defined using an A record to resolve to a single resource always without any specific rules. For instance, a DNS record can be created to resolve the domain to an ALIAS record that routes the traffic to an ELB load balancing a set of EC2 instances.

simple-routing-policy

Weighted Routing Policy

Weighted Routing Policy is used when there are multiple resources for the same functionality and the traffic needs to be split across the resources based on some predefined weights.

weighted-routing-policy

Latency Routing Policy

Latency Routing Policy is used when there are multiple resources for the same functionality and you want Route 53 to respond to DNS queries with answers that provide the best latency i.e. the region that will give the fastest response time.

latency-routing-policy

Failover Routing Policy

Failover Routing Policy is used to create Active/Passive set-up such that one of the site is active and serve all the traffic while the other Disaster Recover (DR) site remains on the standby. Route 53 monitors the health of the primary site using the health check.

Failover Routing.png

Geolocation Routing Policy

Geolocation Routing Policy is used to route the traffic based on the geographic location from where the DNS query is originated. This policy allows to send the traffic to resources in the same region from where the request was originated i.e. it allows to have site affinity based on the location of the users.

geolocation-routing
