# EXAMPLE_1

**What**: 

  - Register a `xburser.com` domain;
  - Create S3 bucket `xburser.com`;
  - Make a Lamda function that returns your name;
  - Place to S3 bucket `*.html` files that will use Lambda to get your name;
  - Make S3 bucket available via the domain specified.
  
  
## 1) REGISTER A DOMAIN

Make a domain registration for `xburser.com`.



## 2) CREATE A S3 BUCKET

  1. Create a new bucket `xburser.com` (the name MUST be the same as domain)

  2. Make it Static-Website.

  3. Put necessary '*.html' files to the bucket.

  4. Make them public.



## 3) MAKE A LAMBDA FUNC THAT RETURNS YOUR NAME

**3.1) Create a function**

Go to `Labmda` service.

Hit `Create a function` button.

```
Name: xbs_lambda_func
Runtime: Python 3.6
Role: Create a new role from template(s)
Role name: xbs_lambda_role
Policy templates: Simple Microservices permissions
```

Hit `Create function` button.

Open just created `xbs_lambda_func` function.

Update `Functin code` area with necessary code. For instance:
```
def lambda_handler(event, context):
    print("In lambda handler")
    
    resp = {
        "statusCode": 200,
        "headers": {
            "Access-Control-Allow-Origin": "*",
        },
        "body": "Sergii Burtovyi"
    }
    
    return resp
```

Hit `Save` button.


**3.2) Configure just created function**

Add `API Gateway` as a trigger.

```
API: Create a new API
Security: 
API name: xbs_lambda_func-API
Deployment stage: prod
```

Hit `Add` button.

Hit `Save` button.


**3.3) Configure API**

Hit `xbs_lambda_func-API` link.

Highlight `ANY` method. 

Click `Actions`, select `Delete Method` from drop-down list, hit `Delete` button.

Click `Actions`, select `Create Method` from drop-down list, select `Get` from drop-down list, confirm it.

```
Integration type: Lambda Function
Use Lambda Proxy Integration: IS checked
Lambda Region: us-east-1   <== region where your lambda function is created
Lambda Function: xbs_lambda_func
Use Default Timeout: IS checked
```

Hit `Save` button. 

Hit `Ok` button.


**3.4) Deploy API**

Click `Actions`, select `Deploy API` from drop-down list, hit `Delete` button.

```
Deployment Stage: prod
Deployment Description: My first testing
```

Click `Deploy` button.

**3.5) Verify triggering your Lambda via API**

Click `Stages`, then `prod`, then `GET`, then hit Invoke URL link.


## 4) PLACE NECESSARY `*.html` FILES TO S3

Place to S3 bucket `*.html` files that will use Lambda to get your name.

Create 



## 5) MAKE A MAPPING DOMAIN -> S3

Go to `Route53` service.

Go to `Hosted Zones`. Click `xburser.com` hosted zone.

Hit `Create Record Set` button. 

```
  Name: xburser.com     <== no prefix (www) here
  Type: A - IPv4 address
  Alias: Yes radiobutton is selected
  Alias Target: put mouse pointer, appropriate line xburser.com (s3-website) has to be available in drop-down list for S3 website endpoints
```

Left other default.

Hit `Create` button.


## VERIFYING

Wait some small time until DNS will be updated.

Try to visit just created domain: [xburser.com](http://xburser.com)











































































