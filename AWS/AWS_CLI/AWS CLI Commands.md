# AWS CLI COMMANDS

## DOCS

[Command Refference](https://docs.aws.amazon.com/cli/latest/index.html)


## S3

[S3 Command Refference](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)

### BUCKETS

List buckets
```
aws s3 ls

      2018-08-19 10:29:22 xburser
      2018-08-19 10:28:33 xburserlondon
```

Copy bucket `xburser` to bucket `xburser london`
```
aws s3 cp --recursive s3://xburser s3://xburserlondon
```
























