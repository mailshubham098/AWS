> #### [Amazon Elastic Container Registry - ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

- Amazon Elastic Container Registry (Amazon ECR) is a fully managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images.
- stores both the container you create and any container software you buy through AWS marketplace
- stores container images in S3
- ability to define and organize repositories in your registry using namespaces
- support to transfer your container images to and from ECR via HTTPS

Components of Amazon ECR

- Registry
  - An Amazon ECR registry is provided to each AWS account; you can create image repositories in your registry and store images in them
- Authorization token
  - Your Docker client must authenticate to Amazon ECR registries as an AWS user before it can push and pull images. The AWS CLI get-login command provides you with authentication credentials to pass to Docker.
- Repository
  - An Amazon ECR image repository contains your Docker images
- Repository policy
  - You can control access to your repositories and the images within them with repository policies.
- Image
  - You can push and pull Docker images to your repositories. You can use these images locally on your development system, or you can use them in Amazon ECS task definitions.

Amazon ECR Lifecycle Policies
- Amazon ECR lifecycle policies enable you to specify the lifecycle management of images in a repository. A lifecycle policy is a set of one or more rules, where each rule defines an action for Amazon ECR.
- You should expect that after creating a lifecycle policy the affected images are expired within 24 hours.
- Lifecycle Policy Parameters
  - Rule Priority
  - Description
  - Tag Status
  - Tag Prefix List
  - Count Type
  - Count Unit
  - Count Number
  - Action

Amazon ECR Interface VPC Endpoints (AWS PrivateLink)
- You can improve the security posture of your VPC by configuring Amazon ECR to use an interface VPC endpoint.
- Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon ECR APIs by using private IP addresses
-  PrivateLink restricts all network traffic between your VPC and Amazon ECR to the Amazon network.
- Also, you don't need an internet gateway, a NAT device, or a virtual private gateway.
- For Amazon ECS tasks using the Fargate launch type, this enables the task to pull private images from Amazon ECR without the need to assign a public IP address to the task.

Limits

| Resource | Default Limit |
| --- | --- |
| Maximum number of repositories per region | 1,000 |
| Maximum number of images per repository | 1,000 |
| Number of API transactions per second, per region, per account | 20 sustained, with the ability to burst up to 200 \* |
| Number of docker pull transactions to a repository per second, per region, per account | 200 sustained, with the ability to burst up to 400 \*  |
| Number of docker pull layer transactions to a repository per second, per region, per account | 200 sustained, with the ability to burst up to 400 \* |
| Number of docker push transactions to a repository per second, per region, per account | 10 sustained, with the ability to burst up to 40 \* |


| Resource | Default Limit |
| --- | --- |
| Maximum number of layers per image | 127 \(this is the current Docker limit\) |
| Maximum number of tags per image | 100 |
| Maximum layer size \*\* | 10,000 MiB |
| Maximum layer part size | 10 MiB |
| Minimum layer part size | 5 MiB \(except the final layer part in an upload\) |
| Maximum number of layer parts | 1,000 |
| Maximum number of rules in a lifecycle policy | 50 |
| Maximum length of a lifecycle policy | 30720 |
