# AWS CLOUDFRONT CDN


## OVERALL

**Content Delivery Network (CDN)** - system of distributed servers (network) that delivers web pages and other web content to the user based on the geographic locations of the user, the origin of the web pages and content delivery server.

### EDGE LOCATION

**Edge location** - location where content will be cached.

### ORIGIN

**Origin** - origin of all the files that the CDN will distribute:
  - S3 Bucket;      <== most common situation
  - EC2 Instance;   <== most common situation
  - Elastic Load Balancer;
  - Route 53.
  - it can be origin even out of AWS (your local one)
  
  
### DISTRIBUTION

**Distribution** - the name given the CDN which consists of a collection of Edge Locations. There are:
  - Web Distribution - typically used for Websites
  - RTMP - used for media streaming.
  
**1) Web Distribution**

Create a web distribution if you want to:

  - Speed up distribution of static and dynamic content, for example, .html, .css, .php, and graphics files.
  - Distribute media files using HTTP or HTTPS.
  - Add, update, or delete objects, and submit data from web forms.
  - Use live streaming to stream an event in real time.

You store your files in an origin - either an Amazon S3 bucket or a web server. After you create the distribution, you can add more origins to the distribution
  
**2) RTMP**

Create an RTMP distribution to speed up distribution of your streaming media files using Adobe Flash Media Server's RTMP protocol. An RTMP distribution allows an end user to begin playing a media file before the file has finished downloading from a CloudFront edge location. Note the following:

  - To create an RTMP distribution, you must store the media files in an Amazon S3 bucket.
  - To use CloudFront live streaming, create a web distribution


  
### TIPS

- Edge locations are not just READ only, you can WRITE to them too
- Objects are cached for the time of TTL (24 hours by default)
- You can clear cached objects before TTL expires, but you have to pay for this option









































