# AWS RDS: AURORA OVERALL

## OVERALL

**Aurora** is a MySQL-compatible, relational DB engine that combines the speed and availability of high-end commercial DBs with the simplicity and cost-effectivness of open-source DBs.

It provides up to 5 times better performance than MySQL at a price point 1/10 that of a commercial DB while delivering similar performance and availability.

_______________________________

Aurora is Amazon's RDS engine.

It can be run only on AWS infrastructure (so it can't be locally installed on your device).

It's MySQL-compatible (so it's easy to do import from MySQL to Aurora).


## SCALING

### RESOURCES

Start with 10GB, scalesin 10GB incrementsto 64TB (Storage is Autoscaling).

Compute  resources can scale up to 32vCPUs and 244GB of Memory.

There's a downtime during scaling (unlike DynamoDB), so it's done during maintenance window.

Aurora storage is self-healing. Data blocks and disks are continuosly scanned for errors and repaired autmatically.

### DATA

2 copies of your data is contained in each AZ with minimum of 3 AZ (so its 6 copies of your data). That's about physical storage media, not actually running instances. If you do wan't to have additional instances, you still need to create Aurora Replicas.

Aurora is designed to transparently handle the loss of up to:
  - 2 copies of data without affecting DB Write availability;
  - 3 copies of data without affecting DB Read availability;



## REPLICAS

2 types of replicas are available:
  - **Aurora Replicas** - Currently you can have up to 15 of them;
  - **MySQL Read Replicas** - Currently you can have up to 5 of them;

Key difference between them is Failover: if you do lose your primary Aurora DB, failover will automatically occure to your Aurora Replica. It would not automatically occure for your MySQL Read Replica.
















