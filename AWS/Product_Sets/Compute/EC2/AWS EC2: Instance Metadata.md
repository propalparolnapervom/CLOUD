# AWS EC2: INSTANCE METADATA

[Instance Metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)


## OVERALL

**Instance metadata** is data about your instance that you can use to configure or manage the running instance. 

Instance metadata is divided into categories.



## EXAMPLES

**List categories**

... with `/` in the end
```
curl http://169.254.169.254/latest/meta-data/

      ami-id
      ami-launch-index
      ami-manifest-path
      block-device-mapping/
      hostname
      iam/
      instance-action
      instance-id
      instance-type
      local-hostname
      local-ipv4
      mac
      metrics/
      network/
      placement/
      profile
      public-hostname
      public-ipv4
      public-keys/
      reservation-id
      security-groups
```

**View specific option**

View EC2 public IP, for example.

... with or without `/` in the end (depending on the list)
```
curl http://169.254.169.254/latest/meta-data/public-ipv4
```
























