# EXAMPLE_2 (Autodiscover via Apache+SSL)

[Docs](https://docs.aws.amazon.com/workmail/latest/adminguide/autodiscover.html)

Setup Amazon WorkMail:
  - for organization `xbs-united` (so e-mail available via browser via [https://xbs-united.awsapps.com/mail](https://xbs-united.awsapps.com/mail))
  - for (already existing) domain `xburser.com` (which looks to your local PC)
  - for user `xburser`
  - AutoDiscover via Apache on your local PC (so Phase 2 from [here](https://docs.aws.amazon.com/workmail/latest/adminguide/autodiscover.html))

(so working e-mail `xburser@xburser.com` has to be as a result)




# AWS SIDE

Configure WorkMail for user `xburser` (as described in example **Example_1**, for instance).

Make sure your domain `xburser.com` contains hosted zone `autodiscover.xburser.com` which points to your local PC as `127.0.0.1`.



# LAPTOP SIDE

[Apache+SSL example](https://www.techrepublic.com/article/how-to-enable-https-on-apache-centos/)

## INSTALL APACHE

Install Apache `httpd` on your local PC.
```
brew install httpd
```



## GENERATE SSL CERTIFICATES 

On your local PC.


### PREPARATION

Install `openssl` utility.
```
brew install openssl
```

Create dirs where SSL certificates will be generated (and stored for backup)
```
mkdir "/Users/sbur/OneDrive - 90percentofeverything.io/keys/test_ssl"
```

Create dirs where SSL certificates will be placed eventally (and linked by `httpd`)
```
sudo mkdir -p /etc/pki/tls/certs
sudo mkdir -p /etc/pki/tls/private
```

### GENERATE SSL CERTIFICATES 

Go to the generation dir
```
cd "/Users/sbur/OneDrive - 90percentofeverything.io/keys/test_ssl"
```

Generate private key
```
openssl genrsa -out ca.key 2048
```

Generate certificate signing request (CSR)
```
openssl req -new -key ca.key -out ca.csr

      You are about to be asked to enter information that will be incorporated
      into your certificate request.
      What you are about to enter is what is called a Distinguished Name or a DN.
      There are quite a few fields but you can leave some blank
      For some fields there will be a default value,
      If you enter '.', the field will be left blank.
      -----
      Country Name (2 letter code) []:UA
      State or Province Name (full name) []:Dnpr
      Locality Name (eg, city) []:Dnp
      Organization Name (eg, company) []:xbs_cmp
      Organizational Unit Name (eg, section) []:it
      Common Name (eg, fully qualified host name) []:xbs_host
      Email Address []:sbur@ciklum.com

      Please enter the following 'extra' attributes
      to be sent with your certificate request
      A challenge password []:xburser!1
```


Generate Self Signed Key
```
openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
```


Put generated certificates to necessary dir (which will be linked by `httpd`).
```
sudo cp ca.crt /etc/pki/tls/certs
sudo cp ca.key /etc/pki/tls/private/ca.key
sudo cp ca.csr /etc/pki/tls/private/ca.csr
```

## CONFIGURE APACHE TO USE SSL

Go to the home dir for `httpd`:
```
cd /usr/local/etc/httpd/
```

Update `httpd.conf` file:

1) to listen 80 and 443 ports
```
Listen 80
Listen 443
```

2) to enable the following Apache modules:
  - proxy
  - proxy_http
  - socache_shmcb
  - ssl
```
  #Uncomment following lines
  
LoadModule socache_shmcb_module lib/httpd/modules/mod_socache_shmcb.so
LoadModule proxy_module lib/httpd/modules/mod_proxy.so
LoadModule proxy_http_module lib/httpd/modules/mod_proxy_http.so
LoadModule ssl_module lib/httpd/modules/mod_ssl.so
```

3) to include generated SSL certificates and to forward to necessary AWS resource:
```
<VirtualHost localhost:443>

SSLEngine on
SSLCertificateFile /etc/pki/tls/certs/ca.crt
SSLCertificateKeyFile /etc/pki/tls/private/ca.key 
 
SSLProxyEngine on 
ProxyPass /autodiscover/autodiscover.xml https://autodiscover-service.mail.us-east-1.awsapps.com/autodiscover/autodiscover.xml

</VirtualHost>
```


# VERIFY

## RESTART APACHE ON YOUR LAPTOP

Start
```
sudo apachectl stop
sudo apachectl start
```

See errors (if any)
```
/usr/local/var/log/httpd/error_log
```

Try to configure your e-mail client now.






































