
|RDS|Aurora|Redshift|DynamoDB|Neptune|DocumentDB|ElasticCache|Amazon QLDB|Amazon Timestream|Amazon Managed Apache Cassandra Service|
|---|------|--------|--------|-------|----------|------------|-----------|-----------------|---------------------------------------|
|Relational|Relational|Relational|Key-Value|graph|Document|In-memory|Ledger Database|Time Series|Wide Column |
||Max Table size = 64TB|
||Global data base support|
||upto - 15 Read replicas |
|MySQL,MariaDB,PostgreSQL,Microsoft SQL Server|MySQL(5x) and PostgreSQL(3x) -compatible|
|Multi-AZ|
|Instance storage : Magnetic,General Purpose (SSD),Provisioned IOPS (PIOPS)|



Relational Database :
- Relational databases store data with predefined schemas and relationships between them. These databases are designed to support ACID transactions, and maintain referential integrity and strong data consistency.
- Traditional applications, ERP, CRM, e-commerce

Key-value Database :
- Key-value databases are optimized for common access patterns, typically to store and retrieve large volumes of data. These databases deliver quick response times, even in extreme volumes of concurrent requests.
- High-traffic web apps, e-commerce systems, gaming applications

In-memory Database :
- In-memory databases are used for applications that require real-time access to data. By storing data directly in memory, these databases deliver microsecond latency to applications for whom millisecond latency is not enough.
- Caching, session management, gaming leaderboards, geospatial applications

Document Database :
- A document database is designed to store semistructured data as JSON-like documents. These databases help developers build and update applications quickly.
- Content management, catalogs, user profiles

Wide Column Database :
- A wide column store is a type of NoSQL database. It uses tables, rows, and columns, but unlike a relational database, the names and format of the columns can vary from row to row in the same table.
- High scale industrial apps for equipment maintenance, fleet management, and route optimization

Graph Database :
- Graph databases are for applications that need to navigate and query millions of relationships between highly connected graph datasets with millisecond latency at large scale.
- Fraud detection, social networking, recommendation engines


Time Series Database :
- Time-series databases efficiently collect, synthesize, and derive insights from data that changes over time and with queries spanning time intervals.
- IoT applications, DevOps, industrial telemetry

Ledger Database :
- Ledger databases provide a centralized and trusted authority to maintain a scalable, immutable, and cryptographically verifiable record of transactions for every application.
- Systems of record, supply chain, registrations, banking transactions
