# EC2 User Data and Metadata

In Amazon EC2, **User Data** and **Metadata** provide ways to manage instance setup and gather instance-specific information.

## User Data
**User Data** is a script that runs when an EC2 instance first starts, allowing for initial configuration tasks, such as installing software or configuring settings. Key points about user data:
- User data is automatically **base64 encoded** when submitted via the AWS Console or AWS CLI.
- It supports both shell scripts and cloud-init directives, which are often used to automate server configuration.

### Example User Data Script
This user data script installs a web server (Apache) and creates a basic webpage:

```bash
#!/bin/bash
# Update the system and install httpd (Apache)
yum update -y
yum install -y httpd

# Start httpd service and enable it to start on boot
systemctl start httpd
systemctl enable httpd
```

## Metadata

Metadata provides information about the EC2 instance itself, such as instance ID, AMI ID, and instance type. Metadata is accessible through a special URL and is divided into different types:

- IMDSv1: The older version, which does not require any token for access.
- IMDSv2: The more secure version, which requires a session token for each metadata request.

## Instance Metadata Service (IMDS)

### IMDSv1

- IMDSv1 is the original version of the Instance Metadata Service.
- It is less secure because it does not require authentication to access metadata.

### IMDSv2

- IMDSv2 improves security by requiring a session token for each metadata request.
- The token has a configurable time-to-live (TTL) in seconds, making it suitable for security-sensitive operations.

## Steps to Access Metadata Using IMDSv2

1. Create a Session and Get a Token

```bash
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
```

2. Use the Token to Request Metadata

- Get the instance ID:

```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id
```

- Get the AMI ID:

```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/ami-id
```

## Combining Metadata with User Data

Using metadata in user data scripts can help customize EC2 instances by retrieving instance-specific information. The following example script installs a web server, retrieves metadata, and displays it on a webpage.

Example Script Using Metadata with User Data

```bash
#!/bin/bash

# Update system and install httpd (Apache)
yum update -y
yum install -y httpd

# Start httpd service and enable it to start on boot
systemctl start httpd
systemctl enable httpd

# Fetch metadata using IMDSv2
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
AMI_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/ami-id)
INSTANCE_TYPE=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-type)

# Create a web page to display the metadata
cat <<EOF > /var/www/html/index.html
<html>
<head>
    <title>EC2 Instance Metadata</title>
</head>
<body>
    <h1>EC2 Instance Metadata</h1>
    <p>Instance ID: $INSTANCE_ID</p>
    <p>AMI ID: $AMI_ID</p>
    <p>Instance Type: $INSTANCE_TYPE</p>
</body>
</html>
EOF
```

This script:

- Installs and configures Apache to serve a webpage.
- Retrieves instance metadata using IMDSv2.
- Displays the metadata in a simple HTML page.

Using metadata with user data enhances the flexibility of EC2 by allowing instance-specific configurations based on dynamically retrieved information.