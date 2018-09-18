# ROOT POLICIES: WEIGHT

## WHAT

  - You've done with Preparation section, so you already have:
    - two EC2 instances (`xbsFrankfurt1` and `xbsFrankfurt2`) in the nearest region (`Frankfurt`) behind the `FrankfurtELB` ELB;
    - one EC2 instance (`xbsVirginia1`) in the farest region (`N.Virginia`) behind the `VirginiaELB` ELB.
  - You already have registered `xburser.com` domain;
  - Create a DNS record with **Weight Routing Policy**.
  
## WEIGHT ROUTING POLICY

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
Routing Policy: Weight
   Weight: 70             <== This is for "FrankfurtELB" in our case.
                              Can be nubmer from 0 to 255. 
                              No traffic for Record Set if 0. 
                              Will be sumed with other Record Set and appropriate Proportion will be resulted.
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
Routing Policy: Weight
   Weight: 30             <== This is for "VirginiaELB" in our case.
                              Can be nubmer from 0 to 255. 
                              No traffic for Record Set if 0. 
                              Will be sumed with other Record Set and appropriate Proportion will be resulted.
   Set ID: xbs in N.Virginia
Evaluate Target Health: No radiobutton is selected.
Associate with Health Check: No radiobutton is selected.
```

Hit `Create` button.

After some time (few minutes), verify your DNS name [http://xburser.com](http://xburser.com) and make sure that its behaiver corresponds to one for `FrankfurtELB` in 70% and to one for `VirginiaELB` in 30%.

> Note: this 70/30 proportion is global. You might don't notice it in a few minutes or on your local PC (because of cached info, for example).
















































