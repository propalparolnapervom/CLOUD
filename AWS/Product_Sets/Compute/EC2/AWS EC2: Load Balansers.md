# AWS LOAD BALANSERS

**Load Balancer** - think of it as Virtual Machine that loads balance for you APP.

## OVERALL

**Types**
  - **Application Load Balancers (ALB)**:
    - Layer 7 (in a smart way);
  - **Network Load Balancers (NLB)**:
    - Layer 4;
    - best suited for Network balancing, where extreme performance is required (millions of requests per second, ultra-low latencies)
  - **Classic Load Balancers (ELB as its old name)**:
    - the legacy Elastic Load Balancers;
    - Layer 7 (a little bit, not so smart as in ALB);
    - Layer 4;


**Errors**
  - **504 Error - The gateway has timed out**: Load balancer is still there, but it is having a problem with communication with EC2 instances behind hit - APP is not responding withing idling timeout period. Troubleshot the APP: is it WebServer or DBServer?
  
If you need IPv4 of your end user, see X-Forwarded-For headers.

**DNS & IP**

You CAN get public DNS name for your ELB, but not IP.

The reason for that is AWS handling public IP for you: it can be changed and you will not notice that.










































