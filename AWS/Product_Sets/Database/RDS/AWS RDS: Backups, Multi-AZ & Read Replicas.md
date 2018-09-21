# AWS RDS: BACKUPS, MULTI-AZ & READ REPLICAS


## BACKUPS

There are 2 different types of Backups for AWS:
  - Automatd backups
  - DB snapshots

### 1) AUTOMATED BACKUPS

**Automated Backups** allow you to recover your DB to any point in time within a "retention period" (1-35 days).

Automated Backup:
  - takes a full daily snapshot;
  - stores transaction logs throughout the day.
  
When you do a DB recovery, AWS will first choose the most recent daily backup and then apply transaction logs relevant to the day. 

This allows you to do a point in time recovery down to a second, withing a retention period.

__________________________

Automated Backups are enabled by default.

The backup data is stored in S3. You get free storage space equal to the size of your DB. 

Backups are taken within a defined window. During the backup window storage I/O may be suspended while your data is being backed up and you may experience elevated latency.




### 2) DATABASE SNAPSHOTS

**DB snapshots** are basicaly snapshots of DB.

It always done manually (i.e. they are user initiated).

They are stored even after you delete the original RDS instance, **unlike Automated Backups**.





## RESTORING BACKUPS

Whenever you restore either Automated Backups or a manual Snapshots, the restored version of the DB will be a new RDS instance with a new DNS endpoint.


## ENCRYPTION 

Encryption at rest is supported for:
  - MySQL;
  - Oracle;
  - SQL Server;
  - PostgreSQL;
  - MariaDB;
  - Aurora.
  
Encryption is done using the **AWS Key Management Service (KMS)** service. 

Once your RDS instance is encrypted, as are its automated backups, read repclicas, snapshots.

At the present time, encrypting an existing DB Instance is not supported. To use Amazon RDS encryption for and existing DB, you must:
  - first create a snapshot;
  - make a copy of that snapshot;
  - encrypt that copy.





## MULTI-AZ

**Multi-AZ** allows you to have an exact copy of your prod DB in another AZ.

AWS handles the replication for you, so when your prod DB is written to, this write will automatically be **Syncronized** to the stand by DB.

In the event of planned DB maintenance, DB failure or AZ failure, RDS will automatically failover to the standby so that DB operations can resume quickly without administrative intervention.

> **Note**: Multi-AZ is for Disaster Recovery only. Not for Performance improving (you need Read Replica for that)

You can turn on Multi-AZ for:
  - SQL Server;
  - Oracle;
  - MySQL;
  - PostgreSQL;
  - MariaDB;
  - Aurora (it's turned on by default, you can not turn it off).



## READ REPLICAS

**Read Replica** allow you to have a read-only copy of your prod DB.

This is achieved by using **Asynchronous** replication from the primary RDS instance to the Read Replica. 

You use Read Replicas primary for very read-heavy DB workloads.

Read Replicas might be done for Read Replicas (so there might be some latency).

You must have Automatic Backups turned on in order to deploy a Read Replica.

You can have up to 5 Read Replicas of any DB.

Each Read Replica will have its own DNS endpoint.

You can have Read Replicas that have Multi-AZ turned on (so Read Replica might be in other AZ).

You can have Read Replica in other Region.

You can ceate Read Replica of source DB with Multi-AZ turned on.

Read Replicas can be promoted to be their own DB. This breaks the replication.



Available for:
  - MySQL Server;
  - PostgreSQL;
  - MariaDB;
  - Aurora.
  
> **Note**: Read Replicas are used for scaling out, not or DR (you have to use Multi-AZ for that).




































