# VPC: NETWORK ACL vs SECURITY GROUPS


## NETWORK ACL

Network ACL can be associated with only 1 VPC.

Subnet can be associated with only 1 NACL.

> NOTE: Subnet MUST be associated with 1 NACL. If you don't associate Subnet with any NACL, it will be associated with default one.

> NOTE: So 2 Subnets of VPC might be associated with different NACL.

AWS recommends to create NACL rules with increment 100

Rule number defines its priority (lower number - more priority). If something is ALLOWED/DENIED by rule with higher priority, the rule that DENIES/ALLOWS it with lower priority will be ignored.

> NOTE: So ensure that you place the DENY rules earlier in the table than the ALLOW rules that open the wide range of ephemeral ports, for example.


NACL created during VPC creation allows all IN/OUT traffic by default . Custom NACL (created manually by you) denies all IN/OUT traffic by default.


### PORTS

Network ACL is **stateless**: if you allow port 80 for IN traffic, it doesn't allow port 80 for 0UT automatically.

> Also, there'is such thing as an [Ephemeral Ports](https://en.wikipedia.org/wiki/Ephemeral_port), when your request on port 80 might be answered on any random port withing some range (security conserns).

That's why you should think about allowing of [AWS Ephemeral Ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports).


See also [Recommended Network ACL Rules for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-recommended-nacl-rules.html)


NACL is placed before Security Group.



## SECURITY GROUP

Security Group is **statefull**: if yu allow port 80 for IN traffic, it allows port 80 for OUT automatically.

> NOTE: It's impossible to block specific IP-address by Security Group. You have to use NACL instead.











































