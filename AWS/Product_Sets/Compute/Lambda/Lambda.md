# LAMBDA

## OVERALL


**AWS Lambda** is an event-driven, serverless computing platform. It is a computing service that runs code in response to events and automatically manages the computing resources required by that code. 

The purpose of Lambda, as compared to AWS EC2, is to simplify building smaller, on-demand applications that are responsive to events and new information. AWS targets starting a Lambda instance within milliseconds of an event. 

AWS Lambda was designed for use cases such as:
  - image or objects uploads to Amazon S3;
  - updates to DynamoDB tables;
  - responding to website clicks;
  - reacting to sensor readings from an IoT connected device. 
  - can also be used to automatically provision back-end services triggered by custom HTTP requests, and "spin down" such services when not in use, to save resources. These custom HTTP requests are configured in AWS API Gateway, which can also handle authentication and authorization in conjunction with AWS Cognito.

Unlike Amazon EC2, which is priced by the hour but metered by the second, AWS Lambda is metered in increments of 100 milliseconds.

Each time you do something with Lambda, Lambda instance is running. 1 user - 1 Lambda instance (althought code might be the same).

Lambda is scaled out instantly automatically (more Lambda instances are added as needed). 

___________________________________________

So:
  - NO SERVERS (no admins for: OS, DB, network, etc - only developers);
  - Continious scaling (instantly, automatically);
  - Super super cheap.

## RUNTIMES AVAILABLE

Following languages are supported:
  - C#
  - Java
  - Node.js
  - Python



## POLICY TEMPLATES

[Docs](https://docs.aws.amazon.com/lambda/latest/dg/policy-templates.html)

When you create an AWS Lambda function in the console using one of the blueprints, Lambda allows you to create a role for your function from a list of Lambda policy templates. By selecting one of these templates, your Lambda function automatically creates the role with the requisite permissions attached to that policy.

The following lists the permissions that are applied to each policy template in the Policy templates list. The policy templates are named after the blueprints to which they correspond. Lambda will automatically populate the placeholder items (such as region and accountID) with the appropriate information. For more information on creating a Lambda function using policy templates, see Create a Simple Lambda Function.

## TRIGGERS

Probably, Core ones to remember:
  - API Gateway
  - CloudWatch Events
  - CloudWatch Logs
  - DynamoDB
  - Kinesis
  - S3
  - SNS

Also:
  - AWS IoT
  - CodeCommit
  - Cognito Sync Trigger
  - SQS


## PRICING

How is Lambda priced?
  1) Number of requests:
    - First million requests are free. $0.20 per 1 million requests thereafter.
  2) Duration:
    - Duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB-second used.
    - there's a limit of 5 minutes: your function CAN'T been executed for 5 mins. If this is the case, you need to divide your function to smaller ones to get one function run another one.
























