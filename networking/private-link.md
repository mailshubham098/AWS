> #### [AWS private link](https://aws.amazon.com/privatelink/)
- AWS PrivateLink simplifies the security of data shared with cloud-based applications by eliminating the exposure of data to the public Internet. AWS PrivateLink provides private connectivity between VPCs, AWS services, and on-premises applications, securely on the Amazon network. AWS PrivateLink makes it easy to connect services across different accounts and VPCs to significantly simplify the network architecture.

Benefits
- Secure your traffic
- Simplify n/w management
- Accelerate your cloud migration

Features
- Accessing services over AWS PrivateLink
 - To use AWS PrivateLink, create an interface VPC endpoint for a service outside of your VPC. This creates an elastic network interface in your subnet with a private IP address that serves as an entry point for traffic destined to the service.
Sharing your services over AWS PrivateLink
- Sharing your services over AWS PrivateLink
  - You can create your own AWS PrivateLink-powered service (endpoint service) and enable other AWS customers to access your service.
- Privately connecting to your on-premises applications
  - Interface VPC endpoints support private connectivity over AWS Direct Connect, so that applications in your premises will be able to connect to these services via the Amazon private network.
- Integration with AWS Marketplace
