# ROOT POLICIES: FAILOVER

## WHAT

  - You've done with Preparation section, so you already have:
    - two EC2 instances (`xbsFrankfurt1` and `xbsFrankfurt2`) in the nearest region (`Frankfurt`) behind the `FrankfurtELB` ELB;
    - one EC2 instance (`xbsVirginia1`) in the farest region (`N.Virginia`) behind the `VirginiaELB` ELB.
  - You already have registered `xburser.com` domain;
  - Create a DNS record with **Failover Routing Policy** (`FrankfurtELB` is primary, `VirginiaELB` is standby).
  
  
  
## FAILOVER ROUTING POLICY

### CREATE A HEALTH CHECKS

Go to `Route53` service.

Click `Health checks` link.


**Health Check for Primary**

Hit `Create health check` button.
```
Name: check_my_primary
What to monitor: Endpoint radiobutton is selected.
Specify endpoint by: Domain name radiobutton is selected.
  Protocol: HTTP
  Domain name: FrankfurtELB-1148726756.eu-central-1.elb.amazonaws.com   <== ELB domain for your primary
  Port: 80
  Path:     <== leave it empty
Request interval: Fast (10 seconds) radiobutton is selected.
Failure threshold: 1
Leave other as default.
```

Hit `Next` button.
```
Create alarm: No radiobutton is selecte.
```

Hit `Create health check` button.



**Health check for whole Website (`xburser.com` domain)**


Hit `Create health check` button.
```
Name: check_my_whole_site
What to monitor: Endpoint radiobutton is selected.
Specify endpoint by: Domain name radiobutton is selected.
  Protocol: HTTP
  Domain name: xburser.com   <== your domain
  Port: 80
  Path: index.html
Request interval: Fast (10 seconds) radiobutton is selected.
Failure threshold: 1
Leave other as default.
```

Hit `Next` button.
```
Create alarm: Yes radiobutton is selecte.
Send notification to: New SNS topic radiobutton is selected
  Topic name: My_domain_down
  Recipient email address: propalparolnapervom@gmail.com
```

Hit `Create health check` button.





### CREATE A FAILOVER ROUTING

**Create root policy for Primary site**

Go to `Route53` service.

Click `Hosted zones`.

Click `xburser.com` domain name.

Delete all Record Sets (if any), except default ones (with `NS` and `SOA` type).

Hit `Create Record Set` button. 
```
Name: xburser.com     <== no "www" here
Type: A - IPv4 address
Alias: Yes radiobutton is selected
Alias Target: select one for your ELB in primary region (which is "FrankfurtELB").
Routing Policy: Failover
Failover Record Type: Primary radiobutton is selected.
Evaluate Target Health: Yes radiobutton is selected.
Associate with Health Check: Yes radiobutton is selected.
  Health Check to Associate: check_my_primary   <== the one you've created previously
```

Hit `Create` button.


**Create root policy for Secondary site**

Hit `Create Record Set` button. 
```
Name: xburser.com     <== no "www" here
Type: A - IPv4 address
Alias: Yes radiobutton is selected
Alias Target: select one for your ELB in secondary region (which is "VirginiaELB").
Routing Policy: Failover
Failover Record Type: Secondary radiobutton is selected.
Evaluate Target Health: No radiobutton is selected.
Associate with Health Check: No radiobutton is selected.
```

Hit `Create` button.

After some time (few minutes), verify your DNS name [http://xburser.com](http://xburser.com) and make sure that its behaiver corresponds to one for `FrankfurtELB` or to one for `VirginiaELB` - depending on availability of Frankfurt.
















































