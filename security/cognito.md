AWS cognito
- we want to give our users an identity so that they can interact with our application
- Cognito user pools:
  - sign in funtionality for app users
  - Integrates with API gateway
- Cognito identity pools (federated identity)
  - provide aws credentials to users so they can access AWS resources directly
  - integrates with cognito user pools as an identity provider
- Cognito Sync
  - synchronizes datra from devices to cognito
  - may be deprecated and replaced by AppSync

AWS Cognito user pools
- create a serverless db of user for your mobile apps
- Simple login : usr (or email)/pwd combination
- Possibility to verify emails/phone numbers and add MFA
- Can enable federated identities (facebook, google, SAML etc)
- Sends back a JSON Web Token (JWT)
- can be integrated with API gateway for authentication

AWS Cognito - Federated Identity pools
- Goal
  - provide direct access to AWS Resources
  - from the Client side
- How
  - log into federated identity provider - or remain anonymous
  - get temporary AWS credentials back from the federated identity pool
  - these credentials come with a pre-defined IAM policy stating their permissions
- Example
  - provide(temporary) access to write to S3 bucket using facebook login

AWS Cognito Sync
- deprecated- use AppSync now
- store preferences,configuration, state of app
- cross device sync ( any platform - iOS,Android etc)
- Offline capability (sync when back online)
- Requires federated identity pool in cognito (not user pool)
- store data in datasets (upto 1 MB)
- Up to 20 datasets to synchronize


Summary
- Cognito user pools:
  - sign in functionality for app users
  - Integrate with API gateway
- Congnito Identity pools (federated identity)
  - provide aws credentials to users so they can acess aws resources directly
  - integrate with cognito user pools as an identity provider
-Cognito sync
  - synchronizes data from device to cognito
  - may be deprecated and replaced by AppSync
