# ROOT POLICIES: GEOLOCATION

## WHAT

  - You've done with Preparation section, so you already have:
    - two EC2 instances (`xbsFrankfurt1` and `xbsFrankfurt2`) in the nearest region (`Frankfurt`) behind the `FrankfurtELB` ELB;
    - one EC2 instance (`xbsVirginia1`) in the farest region (`N.Virginia`) behind the `VirginiaELB` ELB.
  - You already have registered `xburser.com` domain;
  - Create a DNS record with **Geolocation Routing Policy**.
  
  
  
## GEOLOCATION ROUTING POLICY


Go to `Route53` service.

Click `Hosted zones`.

Click `xburser.com` domain name.

Delete all Record Sets (if any), except default ones (with `NS` and `SOA` type).

Hit `Create Record Set` button. 
```
Name: xburser.com     <== no "www" here
Type: A - IPv4 address
Alias: Yes radiobutton is selected
Alias Target: select one for necessary ELB region (which is "FrankfurtELB").
Routing Policy: Geolocation
Location: Europe    <== from where your users will go to Frankfurt in this case
Set ID: europeans
Evaluate Target Health: No radiobutton is selected.
Associate with Health Check: No radiobutton is selected.
```

Hit `Create` button.

After some time (few minutes), verify your DNS name [http://xburser.com](http://xburser.com) and make sure that its behaiver corresponds to one for `FrankfurtELB` or to one for `VirginiaELB` - depending on your geographical location.
















































