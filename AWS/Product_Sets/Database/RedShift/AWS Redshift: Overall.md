# AWS REDSHIFT: OVERALL


## OVERALL

**Redshift** is a fast and powerfull, fully managed, petabyte-scale data warehouse service in the cloud.

Customers can start small for just $0.25 per hour with no commitments or upfront costs and scale to a petabyte or more for $1,000 per terabyte per year, less than a tenth of most other data warehousing solutions.

__________________________________

Datawarehousing is about columns, not rows.

## COLUMNAR DATA STORAGE

Instead of storing data as a series of rows, Redshift organizes the data by column.

Unlike **row-based systems**, which are ideal for transation processing, **column-based systems** are ideal for data warehousing and analytics, where queries often involve aggregates performed over large data sets.

Since only the columns involved in the queries are processed and columnar data is stored sequentially on the storage media, column-based systems require far fewer I/0s, greatly improving query performance.


## ADVANCED COMPRESSION

Columnar data stores can be compressed much more than row-based data stores because similar data is stored sequentially on disk.

Redshift employs multiply compression techniques and can often achieve significant compression relative to traditional relatinal data stores.

In addition, Redshift doesn't require indexes or materialized views and so uses less space than traditional relational DB systems.

When loading data into empty table, Redshift automatically samples your data and selects the most appropriate compression scheme.


## MASSIVE PARALLEL PROCESSING (MPP)

Redshift automatically distributes data and query load across all nodes.

It makes it easy to add nodes to your data warehouse and enables you to maintain fast query performance as your data warehouse grows.



## SECURITY

- Encrypted in transit using SSL
- Encrypted at rest using AES-256 ecncryption
- By default Redshift takes care of key management
  - manage your own keys through HSM
  - AWS Key Management Service


## AVAILABILITY

- Currently only available in 1 AZ (so no spreading between few AZ) (remember: when you talk about datawarehousing, most likely it's not about availability 24/7 as your production DB, but about running some report for management)
- Can restore snapshots to new AZ in the event of outage























