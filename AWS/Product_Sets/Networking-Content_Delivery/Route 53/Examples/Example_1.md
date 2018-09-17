# EXAMPLE_1

**What**: 

  - Register a `xburser.com` domain;
  - Create S3 bucket `xburser.com` with `*.html` files for some Website;
  - Make S3 bucket available via the domain specified.
  
  
## 1) REGISTER A DOMAIN

Make a domain registration for `xburser.com`.



## 2) CREATE A BUCKET

  1. Create a new bucket `xburser.com` (the name MUST be the same as domain)

  2. Make it Static-Website.

  3. Put necessary '*.html' files to the bucket.

  4. Make them public.


## 3) MAKE A MAPPING DOMAIN -> S3

Go to `Route53` service.

Go to `Hosted Zones`. Click `xburser.com` hosted zone.

Hit `Create Record Set` button. 

```
  Name: xburser.com     <== no prefix (www) here
  Type: A - IPv4 address
  Alias: Yes radiobutton is selected
  Alias Target: put mouse pointer, appropriate line xburser.com (s3-website) has to be available in drop-down list for S3 website endpoints
```

Left other default.

Hit `Create` button.


## VERIFYING

Wait some small time until DNS will be updated.

Try to visit just created domain: [xburser.com](http://xburser.com)
































