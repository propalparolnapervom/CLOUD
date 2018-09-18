# ROOT POLICIES: 0) PREPARATION

## WHAT

  - Prepare 2 EC2 instances in `Frankfurt` region:
    - Create `xbsFrankfurt1` and `xbsFrankfurt2` EC2 instances in the `Frankfurt` region;
    - Install `httpd` on both of them;
    - Put appropriate `index.html` on both of them;
    - Create ELB and verify, that it shows different `index.html` after each refresh.
  - Prepare 1 EC2 instance in `N.Virginia` region:
    - Create `xbsVirginia1` EC2 instance in the `N.Virginia` region;
    - Install `httpd` on it;
    - Put appropriate `index.html` on it;
    - Create ELB and verify, that it works.
  
## 1) CREATE EC2 INSTANCES, INSTALL `httpd`, CREATE `index.html`

**Frankfurt Region**

Select necessary region (`Frankfurt`).

Create `xbsFrankfurt1` and `xbsFrankfurt2` EC2 instances.
 
Use default settings.

Use following user data during creation (update it for each EC2 instance):
```
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
echo "<html><body><h1>This is FRANKFURT1</h1></body></html>" > /var/www/html/index.html
```

**N.Virginia Region**

Select necessary region (`N.Virginia`).

Create `xbsVirginia1` EC2 instance.
 
Use default settings.

Use following user data during creation (update it for each EC2 instance):
```
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
echo "<html><body><h1>This is VIRGINIA1</h1></body></html>" > /var/www/html/index.html
```


## 2) CREATE ELB

**Frankfurt Region**

Create `FrankfurtELB` ELB for just created EC2 instances, in the same region and security group.

Use DNS name for ELB, make sure it works for each EC2 instance after each web-page refresh.


**N.Virginia Region**

Create `VirginiaELB` ELB for just created EC2 instance, in the same region and security group.

Use DNS name for ELB, make sure it works.




























