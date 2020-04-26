> #### [Amazon Elastic Container Service ](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

- Amazon Elastic Container Service (Amazon ECS) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster of Amazon EC2 instances.
- you can host your cluster on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks using the **Fargate launch type**.
-  For more control you can host your tasks on a cluster of Amazon Elastic Compute Cloud (Amazon EC2) instances that you manage by using the **EC2 launch type**.
- Amazon ECS eliminates the need for you to operate your own cluster management and configuration management systems or worry about scaling your management infrastructure.
- Amazon ECS can be used to create a consistent deployment and build experience, manage, and scale batch and Extract-Transform-Load (ETL) workloads, and build sophisticated application architectures on a microservices model
-  is a regional service that simplifies running application containers in a highly available manner across multiple Availability Zones within a Region

EB(Elastic-beanstalk) vs ECS
- EB vs ECS really comes down to control. Do you want to control your scaling and capacity or do you want to have that more abstracted and instead focus primarily on your app. ECS will give you control, as you have to specify the size and number of nodes in the cluster and whether or not auto-scaling should be used. With EB, you simply provide a Dockerfile and EB takes care of scaling your provisioning of number and size of nodes, you basically can forget about the infrastructure with the EB route.
- With ECS you'll have to build the infrastructure first before you can start deploying the the Dockerfile so it really comes down to 1) your familiarity with infrastructure and 2) level of effort that you want to spend on the infrastructure vs the app.

Features of Amazon ECS
- Containers and Images
  - application components must be architected to run in containers
  -  Docker container is a standardized unit of software development, containing everything that your software application needs to run: code, runtime, system tools, system libraries, etc. Containers are created from a read-only template called an image.
  - images are then stored in a registry from where they are downloaded
- Task Definitions
  - To prepare your application to run on Amazon ECS, you create a task definition
  - text file, in JSON format, that describes one or more containers, up to a maximum of ten, that form your application
- Tasks and Scheduling
  - A task is the instantiation of a task definition within a cluster.
  - Each task that uses the Fargate launch type has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources, or elastic network interface with another task.
  - ECS task scheduler is responsible for placing tasks within your cluster.
- Clusters
  - When you run tasks using Amazon ECS, you place them on a cluster, which is a logical grouping of resources.
  - When using the Fargate launch type with tasks within your cluster, Amazon ECS manages your cluster resources.
  - When using the EC2 launch type, then your clusters are a group of container instances you manage.
  - An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent
- Container Agent
  - The container agent runs on each infrastructure resource within an Amazon ECS cluster
  - It sends information about the resource's current running tasks and resource utilization to Amazon ECS, and starts and stops tasks whenever it receives a request from Amazon ECS

Cleaning Up your Amazon ECS Resources
- Scale Down Services
  - first scale down the desired count of tasks in these services to 0
- Delete Services
  - Before you can delete a cluster, you must delete the services inside that cluster. After your service has scaled down to 0 tasks, you can delete it
- Deregister Container Instances
  - Before you can delete a cluster, you must deregister the container instances inside that cluster.
- Delete a Cluster
  - After you have removed the active resources from your Amazon ECS cluster, you can delete it.


AWS Fargate on Amazon ECS
- is a technology that you can use with Amazon ECS to run containers without having to manage servers or clusters of Amazon EC2 instances.
- ith AWS Fargate, you no longer have to provision, configure, or scale clusters of virtual machines to run containers. This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing.
- When you run your tasks and services with the Fargate launch type, you package your application in containers, specify the CPU and memory requirements, define networking and IAM policies, and launch the application
- Each Fargate task has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources, or elastic network interface with another task.
- Task Definitions
  - Tasks that use the Fargate launch type do not support all of the task definition parameters that are available
- Network Mode
  - Fargate task definitions require that the network mode is set to awsvpc. The awsvpc network mode provides each task with its own elastic network interface.
- Task CPU and Memory
  - Fargate task definitions require that you specify CPU and memory at the task level. Although you can also specify CPU and memory at the container level for Fargate tasks, this is optional. Most use cases are satisfied by only specifying these resources at the task level.
- Logging
  - Fargate task definitions only support the awslogs and splunk log drivers for the log configuration.
- Amazon ECS Task Execution IAM Role
  - There is an optional task execution IAM role that you can specify with Fargate to allow your Fargate tasks to make API calls to Amazon ECR.
- Task Storage
  - When provisioned, each Fargate task receives the following storage. Task storage is ephemeral. After a Fargate task stops, the storage is deleted.
    - 10 GB of Docker layer storage
    - An additional 4 GB for volume mounts. This can be mounted and shared among containers using the volumes, mountPoints, and volumesFrom parameters in the task definition.
- Task Networking
  - Tasks using the Fargate launch type require the awsvpc network mode, which provides each task with an elastic network interface. When you run a task or create a service with this network mode, you must specify one or more subnets to attach the network interface and one or more security groups to apply to the network interface.
- Private Registry Authentication
  - Fargate tasks can authenticate with private image registries, including Docker Hub, using basic authentication. When you enable private registry authentication, you can use private Docker images in your task definitions.
- Clusters
  - Clusters can contain tasks using both the Fargate and EC2 launch types. When viewing your clusters in the AWS Management Console, Fargate and EC2 task counts are displayed separately
- Fargate Task Retirement
  - A Fargate task is scheduled to be retired when AWS detects the irreparable failure of the underlying hardware hosting the task or if a security issue needs to be patched. Most security patches are handled transparently without requiring any action on your part or having to restart your tasks.
  - When a task reaches its scheduled retirement date, it is stopped or terminated by AWS. If the task is part of a service, then the task is automatically stopped and the service scheduler starts a new one to replace it. If you are using standalone tasks, then you receive notification of the task retirement

Amazon ECS Clusters
- The following are general concepts about Amazon ECS clusters :
  - Clusters are Region-specific.
  - Clusters can contain tasks using both the Fargate and EC2 launch types.
  - For tasks using the EC2 launch type, clusters can contain multiple different container instance types, but each container instance may only be part of one cluster at a time.
  - You can create custom IAM policies for your clusters to allow or restrict user access to specific clusters.

Services
- ECS allows you to run and maintain specified number of instances of your task definition simultaneously in a cluster
- you can optionally run your service behind a LB
- There are two deployment strategies in ECS:
  - Rolling update:
    - scheduler replaces the current running version of the container with the latest one
    - the number of tasks ECS adds or removes from a service during rolling update is controlled by the deployment config, which consist of the min and max tasks allowed during deployment
  - Blue/Green Deployment with AWS CodeDeploy
    - allows you to verify a new deployment of a service before sending production traffic to it
    - the service must be configured to use either an application LB or Network LB

Other points
- Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters and then referencing them in your container definition.For tasks that use the Fargate launch type, the only supported method is referencing a Systems Manager Parameter Store parameter. This feature also requires that your task use platform version 1.3.0 or later.For tasks that use the EC2 launch type, both the Secrets Manager secret and Systems Manager Parameter Store parameter methods described are supported. This feature requires that your container instance have version 1.22.0 or later of the container agent. However, it is recommended to use the latest container agent version.
