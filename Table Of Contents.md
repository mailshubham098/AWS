  ###      RDS

1. Basic Definitions.   

2. DB Instances
  * Magnetic  
  * General Purpose (SSD)  
  * Provisioned IOPS (PIOPS) 

    
3. Failover

4. RDS Read Replicas for read scalability
   * Use cases  


5. Promoting a Read Replica to Be a Standalone DB Instance

6. RDS Backups

7. DB Snapshots
   
8. RDS Encryption  
    * On Demand  
      * Read Request Unit  
      * Write Request Unit  

    * Provisioned mode
         
9. Option Groups

10. DB Parameter Groups

11. Storage

12. Purchasing options

13. Backup and restoring 

14. Monitoring

15. Differences Between CloudWatch and Enhanced Monitoring Metrics

16. Configuring Security

17. Other important points  
	 * RDS supports  (TDE) for DB encryption  
       * Read Request Unit   
       * Write Request Unit  
          
   * Also supports IAM Authentication    
   
  
18. Miscellaneous Points


# Dynamo DB  

1. Basic Definitions. 

2. Core Components
  - a). Tables 
  - b) Attribute
  - c) Items  
  

3. Primary key
  - a). Partition Key
  - b). Partition and Sort Key  
  
  
4. Secondary Indexes

5. Dynamo DB streams 

6. Data Types

7. Read Consistency
   - a) Eventual Consistent Reads
   - b) Strong Consistent Reads  
   

8. Read Write Capacity Mode
    - a) On Demand
        - Read Request Unit
        - Write Request Unit
          
    - b) Provisioned mode
         
9. DynamoDB Auto Scaling

10. Reserved Capacity

11. Point-in-Time Recovery for DynamoDB

12. Amazon DynamoDB Encryption at Rest

13. Amazon DynamoDB Transactions- 

14. In-Memory Acceleration with DAX

15. Use Cases for DAX

16. DAX is not ideal for:


# AURORA 

1. Basic details

2. Amazon Arora DB clusters  

   * Primary DB 
   * Read Replicas  
   

3. Types of Arora Endpoints
   * Cluster
   * Reader
   * Custom
   * Instance  
   

4.  High Availability

5. Storage and Reliability
   * Storage Auto-Repair
   * Crash Recovery  
   

6.  Amazon Arora security

7. Amazon Aurora Global database

8. Amazon Arora Serverless
   1. Features
   2. Use Cases
   3. Limitations  


9.  Pricing

10. Monitoring Amazon RDS
  * Automated Monitoring Tools
  * Manual Monitoring Tools

11. Differences Between CloudWatch and Enhanced Monitoring Metrics

12. Other Points


##    COMPUTE  

# AWS Lambda

1. Basic Definitions. 

2. Manage your own compute resources
  *  Amazon EC2
  *  Elastic Beanstalk
  

3. AWS Lambda Concepts and Features  

   a) Function  
   b) Runtimes  
   c) Layers  
   d) Event source  
   e) Downstream resources  
   f) Log streams  
   g) AWS SAM  



4. AWS Lambda Limits  

  * Execution
  * Deployment
	

5. AWS Lambda Configuration

6. Programming Model  

  * Handler
  * Context
  * Logging
  * Exceptions
  * Concurrency


7. AWS Lambda Retry Behavior

    * Event sources that aren't stream-based
      * Synchronous invocation
      * Asynchronous invocation
    * Poll-based event sources that are stream-based  
    * Poll-based event sources that are not stream-based
   
8. Request Rate

9. Automatic Scaling

10. AWS Lambda Function Dead Letter Queues

11. Supported languages

12. Pricing

13. Using AWS Lambda with CloudFront Lambda@Edge

14. With Lambda@Edge, you can build a variety of solutions, for example:

15. With Lambda@Edge, you can build a variety of solutions examples

16. AWS Serverless Application Model (AWS SAM)   

* Benefits
    * AWS SAM Resources (API, Appli, Function, LayerVer, Simple Table)
    * Write Request Unit   
    

* Controlling Access to API Gateway APIs  

 * Lambda authorizers  
 
 * Amazon Cognito user pools  
		 
 * Deploying serverless applications
     * AWS SAM template
		 * AutoPublishAlias:By  
	     * Deployment Preference Type  
  	     * Canary  
  		 * Linear  
  		* All-at-once  
     
17. Other points  
