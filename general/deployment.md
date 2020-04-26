

In-place vs Disposable Method
  - In-place
    - An in-place upgrade involves performing application updates on live Amazon EC2 instances
    - CodeDeploy and OpsWork
  - Disposable
    -  involves rolling out a new set of EC2 instances by terminating older instances.
    - ElasticBeanStalk, CloudFormation and OpsWork

Blue-Green Method
- Blue-green is a method in which you have two identical stacks of your application running in their own environments. You use various strategies to migrate the traffic from your current application stack (blue) to a new version of the application (green). This is a popular technique for deploying applications with zero downtime. The deployment services like AWS Elastic Beanstalk, AWS CloudFormation, or AWS OpsWorks are particularly useful as they provide a simple way to clone your running application stack. You can set up a new version of your application (green) by simply cloning current version of the application (blue).    

CodeDeploy
- coordinates application deployments across Amazon EC2 instances
- works with your existing application files and deployment scripts, and it can easily reuse existing configuration management scripts.
-  a good choice if you want to deploy code to infrastructure managed by yourself or other people in your organization.
- is a recommended adjunct to AWS CloudFormation for managing the application deployments and updates
- doesn't provision infrastructure
- not autoscalable
- useful for in-place deployment

OpsWork
- AWS OpsWorks provides a simple and flexible way to create and manage stacks and applications. With AWS OpsWorks, you can provision AWS resources, manage their configuration, deploy applications to those resources, and monitor their health.
- AWS OpsWorks is a configuration management service that helps you configure and operate applications in a cloud enterprise by using Puppet or Chef.
- useful for in-place deployment

ElasticBeanStalk
- With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications. Elastic Beanstalk reduces management complexity without restricting choice or control. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.


CloudFormation
- Infrastructure as code
- Using templates means you can impose version control on your infrastructure and easily replicate your infrastructure stack quickly and with repeatability


ElasticBeanStalk vs CloudFormation
- They're actually pretty different. Elastic Beanstalk is intended to make developers' lives easier. CloudFormation is intended to make systems engineers' lives easier.
- Elastic Beanstalk is a PaaS-like layer ontop of AWS's IaaS services which abstracts away the underlying EC2 instances, Elastic Load Balancers, auto scaling groups, etc. This makes it a lot easier for developers, who don't want to be dealing with all the systems stuff, to get their application quickly deployed on AWS. It's very similar to other PaaS products such as Heroku, EngineYard, Google App Engine, etc. With Elastic Beanstalk, you don't need to understand how any of the underlying magic works.
- CloudFormation, on the other hand, doesn't automatically do anything. It's simply a way to define all the resources needed for deployment in a huge JSON file. So a CloudFormation template might actually create two ElasticBeanstalk environments (production and staging), a couple of ElasticCache clusters, a DyanmoDB table, and then the proper DNS in Route53. I then upload this template to AWS, walk away, and 45 minutes later everything is ready and waiting. Since it's just a plain-text JSON file, I can stick it in my source control which provides a great way to version my application deployments. It also ensures that I have a repeatable, "known good" configuration that I can quickly deploy in a different region.


Links
http://jayendrapatil.com/aws-elastic-beanstalk-deployment-strategies/
