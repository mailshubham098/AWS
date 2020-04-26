> #### [Amazon Athena]()
Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to setup or manage, and you pay only for the queries you run. To get started, simply point to your data in S3, define the schema, and start querying using standard SQL.

Features
- interactive query service that makes it easy to analyze data directly in Amazon Simple Storage Service (Amazon S3) using standard SQL.
- Athena is serverless, so there is no infrastructure to set up or manage, and you pay only for the queries you run.
- thena scales automatically—executing queries in parallel—so results are fast, even with large datasets and complex queries.
- helps you analyze unstructured, semi-structured, and structured data stored in Amazon S3.
- supports varios formats like CSV, JSON, or columnar data formats such as Apache Parquet and Apache ORC.
- integrates with Amazon QuickSight for easy data visualization
- use Athena to generate reports or to explore data with business intelligence tools or SQL clients connected with a JDBC or an ODBC driver
- integrates with the AWS Glue Data Catalog, which offers a persistent metadata store for your data in Amazon S3. This allows you to create tables and query data in Athena based on a central metadata store available throughout your AWS account and integrated with the ETL and data discovery features of AWS Glue.
- Has a built-in query editor.
- Uses Presto, an open source, distributed SQL query engine optimized for low latency, ad hoc analysis of data.

Partitioning
- By partitioning your data, you can restrict the amount of data scanned by each query, thus improving performance and reducing cost.
- Athena leverages Hive for partitioning data.
- You can partition your data by any key.

Queries
- You can query geospatial data.
- You can query different kinds of logs as your datasets.
- Athena stores query results in S3.
- Athena retains query history for 45 days.
- Athena does not support user-defined functions, INSERT INTO statements, and stored procedures.
- Athena supports both simple data types such as INTEGER, DOUBLE, VARCHAR and complex data types such as MAPS, ARRAY and STRUCT.

Security
- Control access to your data by using IAM policies, access control lists, and S3 bucket policies.
- If the files in the target S3 bucket is encrypted, you can perform queries on the encrypted data itself.

Pricing
- You pay only for the queries that you run. You are charged based on the amount of data scanned by each query.
- You are not charged for failed queries.
- You can get significant cost savings and performance gains by compressing, partitioning, or converting your data to a columnar format, because each of those operations reduces the amount of data that Athena needs to scan to execute a query.

Other points
- Q: How does Amazon Athena store table definitions and schema?
  - Amazon Athena uses a managed Data Catalog to store information and schemas about the databases and tables that you create for your data stored in Amazon S3. In regions where AWS Glue is available, you can upgrade to using the AWS Glue Data Catalog with Amazon Athena. In regions where AWS Glue is not available, Athena uses an internal Catalog.
You can modify the catalog using DDL statements or via the AWS Management Console. Any schemas you define are automatically saved unless you explicitly delete them. Athena uses schema-on-read technology, which means that your table definitions applied to your data in S3 when queries are being executed. There’s no data loading or transformation required. You can delete table definitions and schema without impacting the underlying data stored on Amazon S3.
- Q: What is the difference between Amazon Athena, Amazon EMR, and Amazon Redshift?
  - Query services like Amazon Athena, data warehouses like Amazon Redshift, and sophisticated data processing frameworks like Amazon EMR, all address different needs and use cases. You just need to choose the right tool for the job. Amazon Redshift provides the fastest query performance for enterprise reporting and business intelligence workloads, particularly those involving extremely complex SQL with multiple joins and sub-queries. Amazon EMR makes it simple and cost effective to run highly distributed processing frameworks such as Hadoop, Spark, and Presto when compared to on-premises deployments. Amazon EMR is flexible - you can run custom applications and code, and define specific compute, memory, storage, and application parameters to optimize your analytic requirements. Amazon Athena provides the easiest way to run ad-hoc queries for data in S3 without the need to setup or manage any servers.
