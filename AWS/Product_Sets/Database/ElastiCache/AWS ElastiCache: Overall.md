# AWS ELASTICACHE: OVERALL


## OVERALL

**ElastiCache** is a web service that makes it easy to deploy, operate and scale an in-memory cache in the cloud.

The service improves the performance of web APP by allowing you to retrieve info from fast, managed, in-memory caches, instead of relying entirely on slower disk-based DB.

So if your APP frequently reads the same amount of infrequently changed data, this data can be placed to the cache.

ElastiCache can be used to significantly improve latency and throughput for many read-heavy APP workloads (social networkin, gaming, media sharing, etc) or compute-intensive workloads (such a recommendation engine).

Caching improves APP performance by storing critical pieces of data in memory for low-latency access. 

Cached info may includ the results of I/0-intensive DB queries or the results of computatinally-intensive calculations.


## TYPES OF ELASTICACHE

Types of caching engines for ElastiCache:
  - **Memcached**
    - A widely adopted memory object chaching system. ElastiCache is protocol compliant with Memcached, so popular tools that you use today with existing Memcached envs will work seamlessly with the service.
  - **Redis**
    - A popular open-source in-memory key-value store that supports data structures such as sorted sets and lists. ElastiCache supports Master/Slave replication and Multi-AZ which can be used to achieve cross AZ redundancy.


## TYPICAL USE CASE

Particular DB is under a lot of stress/load. Which service you should use to alleviate this.

**ElastiCache** is a good choice if your DB is particularly read heavy and not prone to frequent changing.

**Redshift** is a good answer if the reason your DB is feeling stress is because management keep running OLAP transactions on it etc.



































