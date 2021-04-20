## 5 Tips for System Design Interview

1. Do not get into detail too fast.
2. Avoid silver bullet(fix mindset)
3. K-I-S-S Keep it small and simple
4. Justify why you want to make a simple choice
5. Aware of current technologies

## Techonologies to use

1. Load Balancing(Nginx) 

   - For Webserver

2. Caching(Memcached, Redis, CDN to cache static assets file)

   - makes around the world fast, PULL/PUSH to CDN Method 

   - For image server
   - For database server(reads)
   - Least Recently Used, Least Frequently Used, First In First Out eviction policy

3. Slave Master Replication

   - for database server(reads)

4. Databases + Indexes

   - RDBMS - MSSQL/MYSQL For relationship and speed of access
   - NoSql scalability - MongoDB, HBase(supports small write and read)
   - Cassandra NoSql - Masterless, low downtime
   - HBase - Minimum five datanodes and one namenode, high maintenance, require the use of JRuby shell and interdependency like ZooKeeper, high learning curve

5. Database Sharding

   - For database server(write)
   - Horizontal (consistent hashing or hash on keys)/Vertical Sharding (each server has different tables)

6. Load Balancer

   - Determine what algo to use like Round Robin, Weighted Round Robin, Consistent Hashing

7. Pagination

## Basic Steps for Systems Design

1. Determine the functional requirements
2. Determine the non-functional requirements (high availability, data should not be lost)
   - data consistency (seeing same message for all devices)
   - availability (replication)
   - reliability (persistent)
   - latency
3. Determine extended requirements(accessible through REST API's, analytics, push events etc)
4. Limit on content/text (how many data/string can a user push)
5. Traffic estimates
   - Users
   - Files per user
   - Storage per file
6. Storage estimates
   - How long to keep the data
   - char(1 byte), int(4 byte), date(4 byte)
7. Bandwidth estimates
8. Memory estimates
9. System APIs (Parameters/Function calls)
10. Database design (scheme)
    - determine how much each row would be and 
    - if there is relation
    - read/write heavy

## Limitations and solutions

1. Maximum 500 connections for webservers(it will block)
   - split read and write servers and have dedicated services for each other to ensure the system does not block, also allows scaling and optimizing
2. Reliability and redundancy
   - multiple replicas and at least 1 replica in case primary has an issue
   - must be a number of 3 if using master-slave approach
3. Data sharding
   - Determine number of shards to use and use UserID % 10
   - For hot users, we can partition based on photoID, by having a dedicated separate database instance to generate auto-incrementing IDs. To also solve the issue of Single Point of Failure, we can have two KGS with one generating odd and the other generating even and put a round robin infront.
4. Sending Data to Users
   - Pull, clients can pull contents from the server on a regular basis:
     - New data will not be shown until client issue a pull request
     - Most of the time pull request will result in an empty response if there is no new(difference) data
   - Push, maintaining a long poll request with server to receive updates
     - Server has to push update frequently
   - Hybrid:
     - Separate pool for different users
5. Reading data
   - We can maintain latency by pushing contents to cache servers/CDNs
   - Use Cache between Client-Server, Server-Database
   - In addition, for hot users

## Summary
My framework for system design: 1st clarify the domain to design data model, from there design the CRUD API, from there figure out data flow, R/W frequency, persistency requirements which naturally leads to operations/scaling. In short:  
1. Data Model
2. API
3. Data flow
4. Scaling

# Watch In Order
1. https://www.youtube.com/watch?v=vvhC64hQZMk Designing WhatsApp
  - ![image](https://user-images.githubusercontent.com/8999633/115363137-c7bbd500-a1f4-11eb-8429-c6214e93d97d.png)

3. https://www.youtube.com/watch?v=oUJbuFMyBDk Queue
4. https://www.youtube.com/watch?v=FMhbR_kQeHw Publisher Subscriber Model, drawbacks is consistency
5. https://www.youtube.com/watch?v=GeGxgmPTe4c Distributed consensus
  - ![image](https://user-images.githubusercontent.com/8999633/115142621-602b4b80-a075-11eb-94cd-e0988e0eb0c7.png)
6. https://www.youtube.com/watch?v=xrizarXJgC8 Avoid cascading failures in a distributed system
![image](https://user-images.githubusercontent.com/8999633/115150565-82d05b00-a09b-11eb-94d3-cd83a2efccac.png)
  - Caching
  - Gradual deployment
  - Coupling(etc. Save authentication in server memory and assume it works for the next few hours)
6. https://www.youtube.com/watch?v=K0Ta65OqQkY What is Load Balancing?
