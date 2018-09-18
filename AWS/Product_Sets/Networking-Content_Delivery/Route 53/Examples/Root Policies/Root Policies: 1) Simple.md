# ROOT POLICIES: SIMPLE

## WHAT

  - You've done with Preparation section, so you already have:
    - two EC2 instances (`xbsFrankfurt1` and `xbsFrankfurt2`) in the nearest region (`Frankfurt`) behind the `FrankfurtELB` ELB;
    - one EC2 instance (`xbsVirginia1`) in the farest region (`N.Virginia`) behind the `VirginiaELB` ELB.
  - You already have registered `xburser.com` domain;
  - Create a DNS record with **Simple Routing Policy**.
  
## SIMPLE ROUTING POLICY

Go to `Route53` service.

Click `Hosted zones`.

Click `xburser.com` domain name.

Delete all Record Sets (if any), except default ones (with `NS` and `SOA` type).

Hit `Create Record Set` button. 
```
Name: xburser.com     <== no "www" here
Type: A - IPv4 address
Alias: Yes radiobutton is selected
Alias Target: select one for your ELB in nearest region.
Routing Policy: Simple
```

Hit `Create` button.

After some small time (minute or two), verify your DNS name [http://xburser.com](http://xburser.com).
















































