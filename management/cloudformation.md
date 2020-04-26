> ####  [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)



Infrastructure as code
- No resources are created manually which is excellent for control
- The code can be version controlled eg in git
- Changes to the infrastructure are reviewed through code

Cost
- Each resources within the stack is tagged with an identifier so you can easily identify how much a stack costs you
- You can estimate the cost of your resources using cloudformation template

Productivity
- Ability to destroy and re-create an infrastructure on the cloud on the fly
- Automated generation of Diagram for you templates
- Declarative programing (no need to figure out ordering or orchestration)

Separation of concern: create many stacks for many apps and many layers, Eg
  - VPC stacks
  - N/W stacks
  - App stacks

Don't re-invent the wheel
  - Leverage existing templates on the web!
  - Leverage the documentation

How it works
  - Templates has to be uploaded to S3 and then referenced in cloudformation
  - To update , we can edit the previous ones.We have to re-upload the new version of the template
  - Stacks are identified by the name
  - Deleting a stack , deletes every single artifact that was created by CloudFormation

Concepts
- Templates
- Stacks
- Change sets

AWS CloudFormation Best Practices
- Planning and organizing
  - Organize Your Stacks By Lifecycle and Ownership
  - Use Cross-Stack References to Export Shared Resources
  - Use IAM to Control Access
  - Reuse Templates to Replicate Stacks in Multiple Environments
  - Verify Quotas for All Resource Types
  - Use Nested Stacks to Reuse Common Template Patterns

- Creating templates
  - Do Not Embed Credentials in Your Templates
  - Use AWS-Specific Parameter Types
  - Use Parameter Constraints
  - Use AWS::CloudFormation::Init to Deploy Software Applications on Amazon EC2 Instances
  - Use the Latest Helper Scripts
  - Validate Templates Before Using Them

- Managing stacks
  - Manage All Stack Resources Through AWS CloudFormation
  - Create Change Sets Before Updating Your Stacks
  - Use Stack Policies
  - Use AWS CloudTrail to Log AWS CloudFormation Calls
  - Use Code Reviews and Revision Controls to Manage Your Templates
  - Update Your Amazon EC2 Linux Instances Regularly


- AWS CloudFormation Designer (Designer) is a graphic tool for creating, viewing, and modifying AWS CloudFormation templates
