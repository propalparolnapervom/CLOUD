# AWS ROUTE53: ROUTING POLICIES


## SIMPLE

**Simple**: Route53 responds to queries based only on the values in this record. 

This is the default routing policy when you create a new record set.

This is most commonly used when you have a single resource that performs a given function for your domain.

> For example, one web server that serves content for the [http://ukr.net](http://ukr.net) website.


## WEIGHTED

**Weighted**: Route53 responds to queries based on Weighting that you specify in this and other record sets that have the same name and type.

It lets you split your traffic based on different weights assigned.

You might not see this in a few minutes, you can notice result during the whole day.

> For example, you can set 10% of your traffic to go to ELB1 and other 90% - to ELB2 (they might be in different regions or in one).



## LATENCY

**Latency**: Route53 responds to queries based on Regions that you specify in this and other record sets that have the same name and type.



## FAILOVER

**Failover**: Route53 responds to queries using primary record sets if any are healthy, or using secondary record sets otherwise.



## GEOLOCATION

**Geolocation**: Route53 responds to queries based on the locations from which DNS queries originate. We recommend that you create a Default location resource record set.













































