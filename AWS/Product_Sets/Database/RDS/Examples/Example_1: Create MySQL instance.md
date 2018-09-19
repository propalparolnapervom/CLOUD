# EXAMPLE_1

## WHAT
  - Create MySQL instance;
  - Create EC2 instance;
  - Create php code that will be place on EC2 instance to connect to MySQL instance;


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
VPC security groups: Create new VPC security group radiobutton is selected      <== make it new, not existing one
```

```
Database name: xbsdb
Port: 3306
...
```

All other by default.

Hit **Create database** button.

View instance details. Remember connection endpoint:
```
xbsinstance.c9t8cekamnbz.eu-central-1.rds.amazonaws.com
```


## CREATE EC2 INSTANCE

**Place connection php example to S3**

Create some php code that checks connection to MySQL.

Name it `connect.php`.
```
<?php
$username = "xbsadmin";
$password = "xbsadmin";
$hostname = "xbsinstance.c9t8cekamnbz.eu-central-1.rds.amazonaws.com";
$dbname = "xbsdb";

//connection to the database
$dbhandle = mysql_connect($hostname, $username, $password) or die("Unable to connect to MySQL");
echo "Connected to MySQL using username - $username, password - $password, host - $hostname<br>";
$selected = mysql_select_db("$dbname",$dbhandle)   or die("Unable to connect to MySQL DB - check the database name and try again.");
?>

```

Place it to `xburser.com` S3 bucket. Make it public.



**Create EC2 instance to connect from**

Create it in Security Group with only opened SSH and HTTP ports.
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



## VERIFY

Now you have:
  - MySQL instance up and running, in its own Security Group;
  - EC2 instance up and running, in its own Security Group;
  - `connect.php` script on EC2 instance, that is trying to connect to the MySQL instance;

Try to run `connect.php` script: [http://<your_ec2_ip>/connect.php](http://your_ec2_ip/connect.php)

It fails. Because of the fact that MySQL and EC2 instances are in different Security Groups.

**Update Security Group for MySQL to become available**

Go to **RDS** service.

Click **Instances**. Click your instance (**xbsinstance**).

Go to **Connect** area, **Security group rules** block.

Click on the one that currently in use. 

Go to the **Inbound** tab.

Make port 3306 available for your EC2 instance (Source: choose security group of your EC2 instance, make it 0.0.0.0/0, etc).

Try to run `connect.php` script once again: [http://<your_ec2_ip>/connect.php](http://your_ec2_ip/connect.php).

Now connection should be successfull as MySQL and EC2 instances might communicate now (security group of MySQL allows it).











































