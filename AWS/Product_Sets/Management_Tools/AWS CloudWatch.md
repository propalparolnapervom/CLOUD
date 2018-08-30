# AWS CLOUDWATCH



Difference between **CloudWatch** and **CloudTrail**:
  - **CloudWatch**: for performance monitoring/logging of your resources withing AWS (CPU util, Disk I/O, etc)
  - **CloudTrail**: for activity monitoring - what people doing with your AWS account - like creation of the new user/role/S3_bucket/etc;


## OVERALL

AWS CloudWatch is not only for EC2, but for any AWS service that is supported by CloudWatch.

The service should be in use in order to CloudWatch to see it.

**Standard monitoring** = 5 min.

**Detailed monitoring** = 1 min.




## DASHBOARDS

**Dashboard** shows you what is happening with your AWS env.



## ALARMS

**Alarms** allow you to set Alarms that notify you when a particular threshold are hit:
  - billing
  - CPU
  - disk 
  - etc
  
  
## METRICS

Metrics available by default:
  - CPU-related:
    - util;
    - credit usage;
    - credit balance;
    - etc;
  - disk-related:
    - read/write ops;
    - read/write bytes;
  - network-related:
    - in/out;
    - packets in/out;
  
All other metrics have to be customized (additional coding has to be done):
  - RAM
  - EBS Volume utilization
  - etc
  
  
  
## EVENTS

**Events** help you to respond on State changes.

When AWS resources change State, they automatically send Events to the Events stream, you create Rules that create actions on Events.

For example, you can configure rules to:
  - Automatically invoke an AWS Lambda function to update DNS entries when an event notifies you that Amazon EC2 instance enters the Running state
  - Direct specific API records from CloudTrail to a Kinesis stream for detailed analysis of potential security or availability risks
  - Take a snapshot of an Amazon EBS volume on a schedule

## LOGS

**CloudWatch** logs help you to aggregate, monitor, store logs.

Installation and configuration of the Agent required.
































