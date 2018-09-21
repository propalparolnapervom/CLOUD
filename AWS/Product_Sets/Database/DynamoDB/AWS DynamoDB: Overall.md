# AWS DYNAMODB: OVERALL

## OVERALL

**DynamoDB** is a fast and flexible NoSQL DB service for all APP that need consistent, single-digit milliseconds latency at any scale.

It is a fully managed DB and supports both document and key-value data models. 

Its flexible data model and reliable performance make it a greate fit for mobile, web, gaming, ad-tech, IoT, and many other APP.

_____________________________

Stored on SSD storage.

Spread across 3 geographically distinct data centers.



## READ CONSISTENCY 

**Eventual Consistent Reads (Default)**

Consistency across all copies of data is usually reached within a second. Repeating a read after a short time should return the updated data (Best Read Performance).

So use it when your APP can wait a second after write to read updates (withing this time data will be spread across 3 data centers).


**Strongly Consistent Reads**

Returns a result that reflects all writes that receive a successful response prior to the read.

So use it when your APP can't wait any second after write and updates have to available immidiately.



## CAPACITY UNITS

Each **DynamoDB Write Capacity Unit** can handle 1 write per second.

Each **DynamoDB Read Capacity Unit** can handle 1 read per second.


## PRICING

You pay for:

  - **Provisioned Throughput Capacity**
    - Write Throughput $0.0065 per hour for every 10 units;
    - Read Throughput $0.0065 per hour for every 50 units;
 
  - **Storage**
    - It costs of $0.25 per Gb per month.

________________________

### Example of calculating price for Throughput

Your APP needs:
  - to perform 1 million writes per day;
  - to perform 1 million reads per day;
  - to store 3Gb of data;
  
  
**How many writes/reads per sec do you need?**
```
1,000,000 (writes or reads)/24 (hours)/60 (minutes)/ 60 (seconds) = 11.6 (writes or reads) per second
```

**How many Capacity Units do you need?**

Each **DynamoDB Write Capacity Unit** can handle 1 write per second.

Each **DynamoDB Read Capacity Unit** can handle 1 read per second.

So to execute 11.6 (writes or reads) per second you need:
```
12 Write Capacity Units
12 Read Capacity Units
```

**How much do necessary Capacities cost?**

Having $0.0065 per hour for each 10 Write Unit and for each 50 Read Units and need in 12 Read/Write Units:
```
Write:  (0.0065/10) $/hour/units * 12 units * 24 hour = $0.1872 per day
Read:   (0.0065/50) $/hour/units * 12 units * 24 hour = $0.0374 per day 
```

## CONCLUSION

DynamoDB can be expensive for Writes, but it's extremally cheap for Reads. 

So you might look into DynamoDB as opposite to RDS when your prod DB:
  - has a lot of Reads, a little Writes;
  - has to be scalable;
  - needs to have really good performance;
  - doesn't need SQL joins, etc

________________

DynamoDB might be scalable online (as Opposite to RDS): Read/Write Capacities might be added/removed as needed.























