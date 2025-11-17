Content Delivery Network
Content Delivery Network (CDN) is a distributed network that delivers the web content to the user from the server that is nearest to the user location. CDN allows the content to be delivered to users very quickly and with minimal latency since the host servers are located quite close to the geographic location of the user.

Many large organizations with data centers in a specific region or country use CDN to cache the content on the CDN network spread throughout the world and thus allowing the customers in different regions or countries to access the website and other content with very less latency.

CloudFront
CloudFront is the web service in the AWS cloud that uses the edge locations to cache the web content. Requests to the content is automatically routed to the edge location which is nearest to the user. CloudFront can distribute the content from the Origin which could be S3 bucket, EC2 instance, elastic load balancer or Route53. CloudFront can also work with any non-AWS origin server to deliver the content from the edge locations.

When the request for the resource is made for the first time, the content is fetched from the origin and is cached on the edge location for subsequent access to the same resource until the TTL (Time To Live) expires. CloudFront is not only used by users for reading the content but could also be used for writing the content directly to the edge locations. The content is then replicated to the origin server.

There are 2 types of distributions that can be created-

Web distribution – Web distribution is used to speed up the distribution of static and dynamic content using HTTP and HTTPS.
RTMP – RTMP is used to speed up the distribution of the streaming media files using Adobe flash media server’s  RTMP protocol. RTMP protocol allows the user to start playing the media file before the file has finished downloading from the CloudFront edge location. The media files must be stored in S3 bucket.
CloudFront has the option to provide secured access to the resources cached on the edge locations. Permissions can be added on the origin such that the resource could only be accessed through the edge location. Signed URLs or signed cookies ensures that only authenticated users can access the content. Restrictions can also be added by specifying the whitelist or blacklist to prevent users from certain countries from accessing the content. The resources can be invalidated on the edge location to refresh the content before the TTL expires.
