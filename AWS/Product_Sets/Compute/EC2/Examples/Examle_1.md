# EXAMPLE_1

## WHAT

  - Create `xbsFrankfurt1` and `xbsFrankfurt2` EC2 instances;
  - Install `httpd` on both of them;
  - Put appropriate `index.html` on both of them;
  - Create ELB and verify, that it shows different `index.html` after each refresh.
  
## 1) CREATE EC2 INSTANCES, INSTALL `httpd`, CREATE `index.html`

Select necessary region.

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

## 2) CREATE ELB

Create `FrankfurtELB` ELB for just created EC2 instances, in the same region and security group.

Use DNS name for ELB, Make sure it works for each EC2 instance after each web-page refresh.




























