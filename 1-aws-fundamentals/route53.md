# Route 53

Route 53 is a managed DNS (Domain Name System)

DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.
* Route 53 Host Zones are GLOBAL

In AWS, the most common records are (will be on exam):
* A: URL(hostname) to IPv4
* AAAA: URL(hostname) to IPv6
* CNAME: URL(hostname) to URL(hostname)
* ALIAS: URL(hostname) to AWS resource. Comes under A type.

Route 53 can use:
* Public domain names you own
* Private domain names that can be resolved by your instances in your VPCs

Route53 has advanced features such as:
* Load balancing (through DNS - also called client load balancing)
* Health checks (although limited…)
* Routing policy: simple, failover, geolocation, geoproximity, latency, weighted

You pay $0.50 per month per hosted zone

Prefer Alias over CNAME for AWS resources (for performance reasons)

* On windows - nslookup
* On mac - dig. You can see the TTL time remaining before the client has to query Route 53 again.

* Each time the client makes a request to DNS (route 53), the response is cache, until the TTL is expired. 
* larger the TTL, more the chances of having an outdated record in your cache and lesser the queries to the DNS (route 53). and vice versa


CNAME vs Alias

CNAME:
*   IT IS PAID. PREFER ALIAS any day.
*   Point  hostname to any other hostname (app.mydomain.com -> blabla.anything.com)
*   Only for Non Root Domain (something.mydomain.com), cannot be mydomain.com

Alias:
*   hostname to AWS resource (app.mydomain.com to blabla.amazomaws.com)
*   works for ROOT and Non-Root domains. YOu have ROOT domain, you use Alias.
*   Free of charge
*   come with Native health checks

Simple Routing Policy:
* When the Route 53 (DNS) sends in multiple IPs for an A request, the browser can pick which IP to route too randomly. That routed IP will be cached till TTL is expired and MAY change to a different IP once the TTL is expired.

Weighted Routing Policy:
* Helpful to split traffic between 2 regions or instances or IPs.
* Route 53 sets weights(percentage) on endpoints returned and Client's traffic is routed based on the percentage.
* Sum of the weights doesn't have to be 100. Can be any number, the %s are computed internally.
* Helpful to test 1% of traffic on new app version for example
* YOu can associate health checks, traffic will not be sent to an instance if its down. 
* YOu cant see the weights when you use the Dig command.

Latency Routing Policy:
* Redirect traffic from the client, to the region that has the least latency, based on proximity.
* Especially useful when latency is of top priority
* Latency is evaluated in terms of user to designated AWS region.
* You use this policy to minimize response time and to keep the latency to the minimum. 

Health Checks:
* YOu can associate health checks, traffic will not be sent to an instance if its down.
* Health checks can have HTTP, HTTPS and TCP (no SSL verification)
* can be integrated with CloudWatch
* Health checks can be integrated with Route 53. The IP returned by A request is verified to be healthy
* There's fast and slow health checks. Fast checks are paid. 
* YOu can configure the health checker IPs that will basically ping the instance. Default is 15 health checkers

Fail Over Routing policy:
* Has Primary & secondary instances. Secondary is Disaster recovery instance for the Primary one. 
* Can only have 1 primary and 1 secondary
* Request from Client -> Route 53 -> Primary if HEalth check pass, else Secondary
* Health checks are mandatory here for the Primary instance. For Secondary its not mandatory.

Geo Location Routing Policy
* The routing is based on user location
* Traffic originating from UK should go to 11.22.33.44, France should go to 22.33.44.55
* There'll be a **default** policy, in case we dont have rules for a specific location.

Multi Value Routing Policy:
* Use when routing traffic to multiple resources
* Want to associate Route 53 health checks with records
* Upto 8 healthy records are returned for each multi value query
* It is not a substitute for having an ELB. It is an upgrade over Simple Routing algorithm. Its client side load balancing
* dig would show all the resources
* browser would pick one of them at random


