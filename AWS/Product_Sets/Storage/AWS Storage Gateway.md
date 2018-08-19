# AWS STORAGE GATEWAY


## OVERALL INFO

[Docs](https://docs.aws.amazon.com/storagegateway/latest/userguide/WhatIsStorageGateway.html)

**AWS Storage Gateway** connects an on-premises software appliance with cloud-based storage to provide seamless integration with data security features between your on-premises IT environment and the AWS storage infrastructure. 

You can use the service to store data in the AWS Cloud for scalable and cost-effective storage that helps maintain data security.

Consider *Storage Gateway* as a Virtual Machine running on your on-premise server.


## TYPES OF STORAGE

  - **File Gateway (NFS)** - to store flat files (word, pdf, etc) directly in S3, accessed through a **Network File System (NFS)** mount point;
  - **Volume Gateway (iSCSI)** - cloud-backed storage volumes that you can mount as **Internet Small Computer System Interface (iSCSI)** devices from your on-premises application servers
    - **Stored volumes** – Entire Dataset is stored on site and is asynchronously backed up to S3;
    - **Cached volumes** – Entire Dataset is stored on S3 and the most frequently accessed data is cached on site;   
  - **Tape Gateway (VTL)** - Virtual Tabe Library -  backup & archiving solution, allows you to create virtual tapes and then send them to S3 and them using Lifecycle policies to send those tapes to Glacier. Uses popular backup applications like NetBackup, Backup Exec, Veem etc.
  






















