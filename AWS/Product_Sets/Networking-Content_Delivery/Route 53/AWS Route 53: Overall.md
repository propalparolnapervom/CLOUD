# AWS ROUTE 53: OVERALL


## OVERALL


The length that a DNS record is cached on either the Resolving Server or the users own local PC = to the value of the **Time To Live (TTL)** in seconds.




## RECORDS

### "A" RECORD

**"A" record** is used by a computer to translate the name of the domain to the IP address. 

It's a fundamental type of DNS record. 

"A" stends for "Address".

> For example, http://www.ukr.net might point to http://123.23.43.234


### CNAME RECORD

A **Canonical Name (CName)** can be used to resolve one domain name to another.

You might have a domain for your mobile users and domain for regular ones. Instead of using separate IP-adress for each domains, you can point one domain to another.

> For example, http://m.ukr.net might point to http://ukr.net

CNAME can't be used for naked domain names (without `www.` section). You can't have a CNAME for http://acloud.guru (no `www.` section), it must be either an A record or an Alias.

### ALIAS RECORD

> Amazon has created it.

**Alias records** are used to map resource record sets in your hosted zone to ELB, CloudFront distributions or S3 buckets that are configured as websites.

**Alias vs. CNAME**

  - Common: you can map one DNS name (www.example.com) to another 'target' DNS name (elb1234.elb.amazonaws.com);
  - Difference: 
    - a CNAME can't be used for naked domain names (without `www.` section). You can't have a CNAME for http://acloud.guru (no `www.` section), it must be either an *A record* or an *Alias*;
     - a CNAME is charged, an Aliase is for free.
  
 So CNAME cant point to ELB address, for example (which is elb1234.elb.amazonaws.com).
 
 Alias resource record sets can save your time because Amazon Route 53 automatically recognizes changes in the record sets that the alias resource record set refers to. 
 
 > For example: Alias resource record `example.com` point to ELB at `lb1-1234.us-east-1.elb.amazonaws.com`. If the IP-address of ELB changes, Route 53 will automatically reflect those changes in DNS answer for `example.com` without any changes to the hosted zone that contains resource record sets for `example.com`































































