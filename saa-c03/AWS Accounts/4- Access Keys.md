# Access Keys

AWS access keys are credentials used by applications and command-line tools to make programmatic requests to AWS. They are not used to sign in to the AWS Management Console.

## Key Characteristics

- An access key consists of an **Access Key ID** and a **Secret Access Key**.
- The Access Key ID identifies the key, while the Secret Access Key is used to cryptographically sign requests.
- The secret key is shown only when the key is created, so it must be stored securely.
- Access keys should use the least required permissions and be rotated regularly.
- AWS recommends using IAM roles and temporary credentials instead of long-term access keys when possible.

<img width="1253" height="700" alt="AWS access key" src="https://github.com/user-attachments/assets/bc7a1f67-44c8-4eb7-bc08-ecbe3e5d4c0f" />
