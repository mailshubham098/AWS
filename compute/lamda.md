> ##### [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

- With AWS Lambda, you can run code without provisioning or managing servers. You pay only for the compute time that you consume—there’s no charge when your code isn’t running. You can run code for virtually any type of application or backend service—all with zero administration. Just upload your code and Lambda takes care of everything required to run and scale your code with high availability. You can set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app.
- you are responsible only for your code
- AWS Lambda manages the compute fleet that offers a balance of memory, CPU, network, and other resources
- If you need to manage your own compute resources,you can use
  - Amazon EC2
  - Elastic Beanstalk
- run functions in a serverless environment to process events in the language of your choice
- Each instance of your function runs in an isolated execution context and processes one event at a time.
- Lambda automatically scales up the number of instances of your function to handle high numbers of events.
- AWS Lambda is SOC, HIPAA, PCI and ISO compliant

- Function
  - A script or program that runs in AWS Lambda.
  - processes an event and returns a response
- Runtimes
  - runtimes allow functions in different languages to run in the same base execution environment
  - runtime sits in-between the Lambda service and your function code
- Layers
  - layers are a distribution mechanism for libraries, custom runtimes, and other function dependencies
  - let you manage your in-development function code independently from the unchanging code and resources that it uses.
- Event source
  - An AWS service, such as Amazon SNS, or a custom service, that triggers your function and executes its logic
  - source of an event to be processed by lamba
- Downstream resources
  - An AWS service, such as DynamoDB tables or Amazon S3 buckets, that your Lambda function calls once it is triggered.
- Log streams
  - Lambda automatically monitors your function invocations and reports metrics to CloudWatch, you can annotate your function code with custom logging statements that allow you to analyze the execution flow and performance of your Lambda function to ensure it's working properly.
- AWS SAM
  -  model to define serverless applications.
  - AWS SAM is natively supported by AWS CloudFormation and defines simplified syntax for expressing serverless resources

AWS Lambda Limits
- Execution
  - Concurrent executions : 1000
  - Function and layer storage : 75GB
  - Function layers : 5
  - Deployment package : 50MB zipped, 250MP unzipped , 3 MB (console editor)
  - Memory allocation : 128 MB - 3008 MB ( 64 increments)
  - Max execution time : 15 mins
  - Disk capacity in the "function container" ( in /tmp) :512MB
- Deployment
  - Lambda function deployment size (compressed .zip): 50MB
  - Size of uncompressed deployment(code+dependencies) : 250 MB
  - Can use the /tmp directory to load other files at startup
  - Size of env variables : 4KB


AWS Lambda Configuration
- Timeout : default 3 secs , max 15 mins
- can set env variables
- Allocated memory ( 128MB to 3GB)  (64MB increments)
- Able to deploy within VPC + assign security groups
- IAM execution role must be attached to the lambda function

Programming Model
- writing code for a Lambda function that includes the following core concepts:
  - Handler
  - Context
  - Logging
  - Exceptions
  - Concurrency

AWS Lambda Retry Behavior
- Lambda handles retries in the following manner, depending on the source of the invocation.
  - Event sources that aren't stream-based
    - Synchronous invocation
      - Lambda includes the FunctionError field in the response body, with details about the error in the X-Amz-Function-Error header. The status code is 200 for function errors.
    - Asynchronous invocation
      - Asynchronous events are queued before being used to invoke the Lambda function. If AWS Lambda is unable to fully process the event, it will automatically retry the invocation twice, with delays between retries. Configure a dead letter queue for your function to capture requests that fail all three attempts.
  - Poll-based event sources that are stream-based
    - These consist of Kinesis Data Streams or DynamoDB. When a Lambda function invocation fails, AWS Lambda attempts to process the erring batch of records until the time the data expires, which can be up to seven days.
    - The exception is treated as blocking, and AWS Lambda will not read any new records from the shard until the failed batch of records either expires or is processed successfully. This ensures that AWS Lambda processes the stream events in order.
  - Poll-based event sources that are not stream-based
    -  This consists of Amazon Simple Queue Service. If you configure an Amazon SQS queue as an event source, AWS Lambda will poll a batch of records in the queue and invoke your Lambda function. If the invocation fails or times out, every message in the batch will be returned to the queue, and each will be available for processing once the Visibility Timeout period expires. (Visibility timeouts are a period of time during which Amazon Simple Queue Service prevents other consumers from receiving and processing the message).
    - Once an invocation successfully processes a batch, each message in that batch will be removed from the queue. When a message is not successfully processed, it is either discarded or if you have configured an Amazon SQS Dead Letter Queue, the failure information will be directed there for you to analyze.

Request Rate
  -  rate at which your Lambda function is invoked
  - request rate = number of concurrent executions / function duration

Automatic Scaling
  - AWS Lambda dynamically scales function execution in response to increased traffic, up to your concurrency limit.

AWS Lambda Function Dead Letter Queues
  - Any Lambda function invoked asynchronously is retried twice before the event is discarded. If the retries fail and you're unsure why, use Dead Letter Queues (DLQ) to direct unprocessed events to an Amazon SQS queue or an Amazon SNS topic to analyze the failure.
  - AWS Lambda directs events that cannot be processed to the specified Amazon SNS topic or Amazon SQS queue. Functions that don't specify a DLQ will discard events after they have exhausted their retries.

Supported languages
  - Java
  - C#
  - Go
  - Python
  - Node.js
  - Ruby

Pricing
- You are charged for total number of requests for your functions and the duration (the time it takes for your code to execute)

Using AWS Lambda with CloudFront Lambda@Edge
- Lambda@Edge lets you run Node.js Lambda functions to customize content that CloudFront delivers, executing the functions in AWS locations closer to the viewer
- The functions run in response to CloudFront events, without provisioning or managing servers.
- You can use Lambda functions to change CloudFront requests and responses at the following points:
  - After CloudFront receives a request from a viewer (viewer request)
  - Before CloudFront forwards the request to the origin (origin request)
  - After CloudFront receives the response from the origin (origin response)
  - Before CloudFront forwards the response to the viewer (viewer response)

With Lambda@Edge, you can build a variety of solutions, for example:
  - Inspect cookies to rewrite URLs to different versions of a site for A/B testing.
  - Send different objects to your users based on the User-Agent header,
  - Inspect headers or authorized tokens, inserting a corresponding header and allowing access control before forwarding a request to the origin
  - Add, delete, and modify headers, and rewrite the URL path to direct users to different objects in the cache.
  - Generate new HTTP responses to do things like redirect unauthenticated users to login pages, or create and deliver static webpages right from the edge.

#### [AWS Serverless Application Model (AWS SAM)](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html)
- The AWS Serverless Application Model (AWS SAM) is an open-source framework that enables you to build serverless applications on AWS. It provides you with a template specification to define your serverless application, and a command line interface (CLI) tool.
- A serverless application can include one or more nested applications.
-
Benefits
  - Single-deployment configuration
  - Extension of AWS CloudFormation.
  - Built-in best practices.
  - Local debugging and testing
  - Deep integration with development tools

-  AWS SAM template file
  - is a YAML or JSON configuration file that adheres to the open source AWS Serverless Application Model specification.
  - extension of AWS CloudFormation templates
- Declaring Serverless Resources
  - AWS SAM defines the following resources that are specifically designed for serverless applications:
    - AWS::Serverless::Api
    - AWS::Serverless::Application
    - AWS::Serverless::Function
    - AWS::Serverless::LayerVersion
    - AWS::Serverless::SimpleTable

Controlling Access to API Gateway APIs
  - Lambda authorizers
    -  is a Lambda function that you provide to control access to your API.
    - When your API is called, this Lambda function is invoked with a request context or an authorization token that are provided by the client application.
  - Amazon Cognito user pools
    - user pools are user directories in Amazon Cognito
    - A client of your API must first sign a user in to the user pool and obtain an identity or access token for the user. Then your API is called with one of the returned tokens. The API call succeeds only if the required token is valid.

Deploying serverless applications
- If you use AWS SAM to create your serverless application, it comes built-in with CodeDeploy to help ensure safe Lambda deployments. With just a few lines of configuration, AWS SAM does the following for you:
  - Deploys new versions of your Lambda function, and automatically creates aliases that point to the new version.
  - Gradually shifts customer traffic to the new version until you're satisfied that it's working as expected, or you roll back the update.
  - Defines pre-traffic and post-traffic test functions to verify that the newly deployed code is configured correctly and your application operates as expected.
  - Rolls back the deployment if CloudWatch alarms are triggered.

- The revisions to the AWS SAM template do the following:
  - **AutoPublishAlias**:By adding this property and specifying an alias name, AWS SAM:
    - Detects when new code is being deployed, based on changes to the Lambda function's Amazon S3 URI.
    - Creates and publishes an updated version of that function with the latest code.
    - Creates an alias with a name that you provide (unless an alias already exists), and points to the updated version of the Lambda function. Function invocations should use the alias qualifier to take advantage of this
  - **Deployment Preference Type**:The following table outlines other traffic-shifting options that are available
    - Canary
      - Traffic is shifted in two increments. You can choose from predefined canary options. The options specify the percentage of traffic that's shifted to your updated Lambda function version in the first increment, and the interval, in minutes, before the remaining traffic is shifted in the second increment.
    - Linear
      - Traffic is shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic that's shifted in each increment and the number of minutes between each increment.
    - All-at-once
      - All traffic is shifted from the original Lambda function to the updated Lambda function version at once.

Other points
- The first time you create or update Lambda functions that use environment variables in a region, a default service key is created for you automatically within AWS KMS. This key is used to encrypt environment variables. However, if you wish to use encryption helpers and use KMS to encrypt environment variables after your Lambda function is created, you must create your own AWS KMS key and choose it instead of the default key. The default key will give errors when chosen. Creating your own key gives you more flexibility, including the ability to create, rotate, disable, and define access controls, and to audit the encryption keys used to protect your data.
- When you create or update Lambda functions that use environment variables, AWS Lambda encrypts them using the AWS Key Management Service. When your Lambda function is invoked, those values are decrypted and made available to the Lambda code.The first time you create or update Lambda functions that use environment variables in a region, a default service key is created for you automatically within AWS KMS. This key is used to encrypt environment variables. However, if you wish to use encryption helpers and use KMS to encrypt environment variables after your Lambda function is created, you must create your own AWS KMS key and choose it instead of the default key. The default key will give errors when chosen. Creating your own key gives you more flexibility, including the ability to create, rotate, disable, and define access controls, and to audit the encryption keys used to protect your data.
