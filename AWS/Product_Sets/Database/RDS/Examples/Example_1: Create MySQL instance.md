# EXAMPLE_1

## WHAT
  - Create MySQL instance


## CREATE MYSQL INSTANCE

Go to **RDS** service.

Select necessary Region.

Hit **Create database** button.

Check **MySQL** radiobutton. Hit **Next** button.
```
DB instance identifier: xbsinstance
Master username: xbsadmin
Master password: xbsadmin
```

Hit **Next** button.
```
Virtual Private Cloud (VPC): Default VPC ...
Subnet group: default
Public accessibility: No radiobutton is selected
Availability zone: No preference
VPC security groups: Create new VPC security group radiobutton is selected
```

```
Database name: xbsdb
Port: 3306
...
```

All other by default.

Hit **Create database** button.



## CREATE EC2 INSTANCE

**Place connection php example to S3**

Place following `connect.php` example file to `xburser.com` S3 bucket:
```
<?php 
$username = "xbsadmin"; 
$password = "xbsadmin"; 
$hostname = "yourhostnameaddress"; 
$dbname = "xbsdb";

//connection to the database
$dbhandle = mysql_connect($hostname, $username, $password) or die("Unable to connect to MySQL"); 
echo "Connected to MySQL using username - $username, password - $password, host - $hostname<br>"; 
$selected = mysql_select_db("$dbname",$dbhandle)   or die("Unable to connect to MySQL DB - check the database name and try again."); 
?>
```



**Create EC2 instance to connect from**

Use following user data
```
#!/bin/bash
yum install httpd php php-mysql -y
yum update -y
chkconfig httpd on
service httpd start
echo "<?php phpinfo();?>" > /var/www/html/index.php
cd /var/www/html
wget https://s3.eu-central-1.amazonaws.com/xburser.com/connect.php

```














































