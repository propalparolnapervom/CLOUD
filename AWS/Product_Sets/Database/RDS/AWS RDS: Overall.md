# AWS RDS: OVERALL



## OLTP VS. OLAP


We can divide IT systems into transactional (OLTP) and analytical (OLAP). 

In general we can assume that OLTP systems provide source data to data warehouses, whereas OLAP systems help to analyze it. 


**OLTP (On-line Transaction Processing)** is characterized by a large number of short on-line transactions (INSERT, UPDATE, DELETE). 

The main emphasis for OLTP systems is put on very fast query processing, maintaining data integrity in multi-access environments and an effectiveness measured by number of transactions per second. 

In OLTP database there is detailed and current data, and schema used to store transactional databases is the entity model (usually 3NF). 


**OLAP (On-line Analytical Processing)** is characterized by relatively low volume of transactions. 

Queries are often very complex and involve aggregations. 

For OLAP systems a response time is an effectiveness measure. 

OLAP applications are widely used by Data Mining techniques. 

In OLAP database there is aggregated, historical data, stored in multi-dimensional schemas (usually star schema). 




## RELATIONAL DB (OLTP)

**Relational DB** - about rows and columns. You need understand all your columns before you create a table.

If we would want to add some additional info for already existing table in RDBMS, it might be necessary to add a whole new column to describe new info.


**RDBMS on AWS**

RDBMS available on AWS:
  - MS SQL;
  - Oracle;
  - MySQL;
  - PostgreSQL;
  - Aurora (Amazon's own RDBMS);
  - MariaDB;
  
  
  
 

## NON RELATIONAL DB (NOSQL DB)

**Non Relational DB** - you don't need to understand all your "columns" before you create a "table": it's totally flexible as "rows" are created with free amount of "columns".

DB:
  - Collection    **<== Table**
  - Document      **<== Row**
  - Key Value Pair  **<== Column** (`id` in following example)

Example of data in DynamoDB (NoSQL) in `JSON` format:
```
{
"id": "2j3jhg4jhg3",
"firstname": "John",
"surname": "Smith",
"Age": 23",
"address": [
            {
            "street": "21 Jump Street",
            "suburb": "Richmond"
            }
           ]
}
```


**NoSQL DB on AWS**

NoSQL DB available on AWS:
  - Dynamo DB (Amazon's own NoSQL DB);




## DATA WAREHOUSING (OLAP)

**Data warehouse** is using for business intelligance.

Used to pull in very large and complex data sets. 

Usually used by management to do queries on data (such as current performance vs targets etc).

Tools:
  - Cognos;
  - Jaspersoft;
  - SQL Server Reporting Services;
  - Oracle Hyperion;
  - SAP NetWeaver;

Amazon's Warehouse:
  - Redshift




## ELASTICCACHE

**ElasticCache** is a web service that makes it easy to deploy, operate and scale an in-memory chache in the cloud.

It improves the performance of web applications by allowing you to retrieve info from fast, managed, in-memory caches, instead of relying entirely on slower disk-based DB.

ElasticCache supports 2 open-source in-memory caching engines:
  - Memcached
  - Redis



































