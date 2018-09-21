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


## PRICING

You pay for:

  - **Provisioned Throughput Capacity**
    - Write Throughput $0.0065 per hour for every 10 units;
    - Read Throughput $0.0065 per hour for every 50 units;
 
  - **Storage**
    - It costs of $0.25 per Gb per month.

________________________

**Example of calculating price for Throughput**





























