# ROOT POLICIES: LATENCY

## WHAT

  - You've done with Preparation section, so you already have:
    - two EC2 instances (`xbsFrankfurt1` and `xbsFrankfurt2`) in the nearest region (`Frankfurt`) behind the `FrankfurtELB` ELB;
    - one EC2 instance (`xbsVirginia1`) in the farest region (`N.Virginia`) behind the `VirginiaELB` ELB.
  - You already have registered `xburser.com` domain;
  - Create a DNS record with **Latency Routing Policy**.
  
## LATENCY ROUTING POLICY

Go to `Route53` service.

Click `Hosted zones`.

Click `xburser.com` domain name.

Delete all Record Sets (if any), except default ones (with `NS` and `SOA` type).

Hit `Create Record Set` button. 
```
Name: xburser.com     <== no "www" here
Type: A - IPv4 address
Alias: Yes radiobutton is selected
Alias Target: select one for your ELB in nearest region (which is "FrankfurtELB").
Routing Policy: Latency
   Region: eu-central-1   <== this is for Frankfurt
   Set ID: xbs in Frankfurt
Evaluate Target Health: No radiobutton is selected.
Associate with Health Check: No radiobutton is selected.
```

Hit `Create` button.


Hit `Create Record Set` button. 
```
Name: xburser.com     <== no "www" here
Type: A - IPv4 address
Alias: Yes radiobutton is selected
Alias Target: select one for your ELB in farest region (which is "VirginiaELB").
Routing Policy: Latency
   Region: us-east-1          <== this is for N.Virginia
   Set ID: xbs in N.Virginia
Evaluate Target Health: No radiobutton is selected.
Associate with Health Check: No radiobutton is selected.
```

Hit `Create` button.

After some time (few minutes), verify your DNS name [http://xburser.com](http://xburser.com) and make sure that its behaiver corresponds to one for `FrankfurtELB` or to one for `VirginiaELB` - depending on latency (so Frankfurt might be faster as it is nearest).

> Note: you can try to use any VPN-client to ckeck if it will be faster to connect to N.Virginia.
















































