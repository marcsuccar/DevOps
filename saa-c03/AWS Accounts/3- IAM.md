# IAM

IAM is a service that helps control who can access your AWS account and what they are allowed to do. It enables you to create multiple identities and assign permissions to them, allowing granular control over access.

## Key Characteristics

- IAM is globally resilient and secure across all AWS regions.
- Each AWS account has its own dedicated instance of IAM, independent of other AWS accounts.

## Key Features of IAM

- **Identity Provider (IDP):** IAM lets you create and manage user identities. It also authenticates users and roles when they attempt to access AWS resources.
- **Authentication:** Users prove their identity using credentials, such as a username and password.
- **Authorization:** After authentication, IAM checks whether the user is authorized to perform specific actions based on the policies attached to their identity.
- **Federation and Multi-Factor Authentication (MFA):** IAM supports federating external identities (such as Google or Active Directory) and adding an additional security layer through MFA.

<img width="1253" height="700" alt="image" src="https://github.com/user-attachments/assets/709f7e86-e4ea-4091-81b1-7771daa0086d" />
