# Using IAM Roles for EC2

## Overview

When configuring an EC2 instance, it's essential to avoid using Access Keys for AWS CLI access. Using access keys on an instance can create security risks, as they might be exposed and could lead to unauthorized access. Instead, the best practice is to use IAM roles for granting necessary permissions to your EC2 instances.

## Why Avoid Access Keys?

Using `aws configure` to set up access with access keys on an EC2 instance can result in:

- Security Vulnerabilities: Access keys can be exposed if stored on the instance and may be misused.
- Management Complexity: If access keys are compromised, they need to be rotated and reconfigured manually, which is more challenging than updating IAM role policies.

## Best Practice: Use IAM Roles for EC2

To securely access AWS services from your EC2 instance, use IAM roles. Here’s how:

### Step 1: Create an IAM Role for EC2

To grant your instance permissions, such as viewing Amazon S3 buckets, follow these steps:

- In the AWS Management Console, go to IAM > Roles.

- Click on Create role and select EC2 as the trusted entity.

- Select the policy that fits your instance’s access needs, e.g., `AmazonS3ReadOnlyAccess` for read-only access to S3.

- Give your role a descriptive name, such as `EC2-S3-ReadOnly-Role`.

- Optionally, add tags to help organize or manage roles.

- Review the permissions and create the role.

### Attach the IAM Role to an EC2 Instance

You then launch your EC2 instance. For the new instance, select the IAM role under IAM role during the setup. Finally, For an existing instance, click Actions > Security > Modify IAM Role, and select the IAM role you created.

You can now verify your Role permission on the instance with the command:

```bash
aws s3 ls
```

This command should work if the assigned IAM role has S3 read permissions.


