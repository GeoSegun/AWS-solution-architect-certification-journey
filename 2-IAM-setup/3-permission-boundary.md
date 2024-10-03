# Permission Boundary in AWS

In this guide, we will explore **Permission Boundaries** in AWS IAM, their importance, and how they help mitigate the risk of **privilege escalation attacks**.



## What is a Permission Boundary?

A **Permission Boundary** is an advanced feature in AWS IAM that restricts the maximum permissions an IAM user or role can have. Even if the user or role is attached to a policy granting broader permissions, the boundary ensures they cannot exceed the permissions defined by the boundary.

## Why Are Permission Boundaries Important?

### Preventing Privilege Escalation Attacks

Without permission boundaries, a user with elevated privileges like `IAMFullAccess` could create or modify other users or roles and grant them excessive permissions, such as `AdministratorAccess`. This could lead to a privilege escalation attack, where a user can bypass the intended restrictions and perform unauthorized actions.

**Example:**

Consider an IAM user, **Segun**, with the following permissions:
- **IAMFullAccess**: This policy allows Segun to create and manage IAM users, roles, and policies.
- **No permission to launch AWS resources** (e.g., EC2 instances).

**Problem Without Permission Boundary:**

- Segun can create a new user or role and assign them the `AdministratorAccess` policy.
- Segun can then log in as this new user with full administrative privileges and bypass the initial restrictions, such as launching EC2 instances or performing other restricted actions like mining Bitcoin.

**Solution with Permission Boundary:**

With a **permission boundary** in place, Segun's actions are limited to what the boundary allows, even if he tries to grant broader permissions to another user or role.

For instance, Segun can only create users or roles with equal or lesser privileges. Even if Segun assigns `AdministratorAccess` to a new user, that new user won't have the full privileges due to the boundary.

## How to Set a Permission Boundary

### Step 1: Define a Permission Boundary Policy

You can define a permission boundary using a managed policy. Below is an example of a permission boundary that allows a user to create new users, but only with `EC2ReadOnlyAccess` permission.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateUser",
                "iam:AttachUserPolicy",
                "iam:PutUserPolicy"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateRole",
                "iam:AttachRolePolicy",
                "iam:PutRolePolicy"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PermissionsBoundary": "arn:aws:iam::aws:policy/EC2ReadOnlyAccess"
                }
            }
        }
    ]
}
```
In this policy:

- CreateUser and AttachUserPolicy allow Segun to create new users and attach policies.
- The Condition block ensures that any new users or roles created by Segun are limited by the EC2ReadOnlyAccess boundary.

### Step 2: Attach the Permission Boundary to a User or Role

To apply a permission boundary:

1. Go to the IAM console.

2. Select Users or Roles and choose the user or role to which you want to apply the boundary.

3. Under Permissions Boundary, select the policy you created earlier (e.g., the EC2ReadOnlyAccess policy).
Once the permission boundary is applied, the user cannot exceed the permissions defined in the boundary, even if broader permissions are granted via other policies.

## Real-World Example of Permission Boundary in Action

Imagine Segun is part of an operations team and is given IAMFullAccess to manage users. However, the company wants to ensure Segun doesn't create users with more permissions than he has or perform critical actions, such as launching EC2 instances.

By applying a permission boundary to Segun's role, even though he has IAMFullAccess, he can only create users with limited access, ensuring that critical operations (e.g., launching EC2 instances) remain outside his control.

## Conclusion

Permission Boundaries are a powerful tool for securing your AWS environment and preventing privilege escalation attacks. By using them, you can limit the maximum actions that IAM users and roles can perform, ensuring tighter control over your AWS resources.