# AWS EC2: SECURITY GROUP



## OVERALL

**Security Group** - think of it as Virtual Firewall. It is controlling traffic to your EC2 instances.

___________________________

By default:
  - All INBOUND traffic is denied;
  - All OUTBOUND traffic is allowed.
  
You can't deny any INBOUND traffic in the Security Groups, you can only allow it.

  
___________________________

**Security Groups are STATEFULL**. 

Everything you allow in INBOUND (HTTP:80, TCP:22) will be allowed OUTBOUND as well (:80, :22, etc) AUTOMATICALLY - *even if OUTBOUND page is empty!!!*

And this is difference with Network Access Control List, which is **STATELESS** (so no automatic OUTBOUND connections are allowed).

___________________________

You can't block specific IP addresses by Security Group (Network ACL do this).



























