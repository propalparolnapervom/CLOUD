## REQUIREMENTS

**What:** Create EC2 instance, place some static html page, make it available to the world.

## EC2 INSTANCE


### 1. Launch EC2 instance


### 2. Make necessary updates/installation

**1. Connect to just created EC2 instance**

From Linux
```
ssh ec2-user@<pub_ip> -i <private_key.pem>
```

**2. Apply all security updates**

Just to be sure that all your packages are up-to-date
```
sudo su
yum update -y
```


## WEB SERVER

**1. Install Apache**

Install Apache software
```
yum install -y httpd
```

Start Apache
```
service httpd start
```

Make sure it starts every time automatically
```
chkconfig httpd on
```

**2. Make necessary dirs**

If not exists yet ...
```
mkdir -p /var/www/html
```

**3. Place necessary html**
```
cd /var/www/html
touch index.html
vi index.html
  
  <html><h1> XBURSER: Test is successfull!!! </h1></html>
```

**4. Check web-page availability**
```
http://52.59.237.102
```













































