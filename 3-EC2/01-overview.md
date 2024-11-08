# Amazon EC2 (Elastic Compute Cloud)

## What is EC2?
Amazon EC2 (Elastic Compute Cloud) is a web service that provides secure, resizable compute capacity in the cloud. It allows users to run applications on virtual servers, known as instances, and scale compute power up or down based on demand.

Amazon EC2 offers flexibility by allowing users to choose various instance types, operating systems, and networking options, making it a popular choice for a wide range of workloads, including web applications, machine learning models, and data analysis.

### Supported Operating Systems
EC2 instances can run various operating systems, with the three main ones being:
- **Linux**: Common for server-side applications, DevOps, and system administration tasks.
- **Windows**: Often used for applications built on the Microsoft stack (e.g., .NET).
- **Mac**: Available for macOS development and testing (typically used for Xcode builds).

## Types of IP Addresses in EC2
Amazon EC2 instances interact with the outside world and other AWS services using different types of IP addresses.

### 1. Public IPs
A **public IP** is an internet-routable IP address that allows the instance to communicate externally over the internet. It’s assigned dynamically to instances within a public subnet and can change when the instance is stopped and restarted.

- **Use Case**: When an EC2 instance needs direct communication with the internet.

### 2. Private IPs
A **private IP** is used within a Virtual Private Cloud (VPC) for internal communication between resources. It is not routable over the internet and remains consistent for the lifetime of the instance.

- **Use Case**: For instances that only need to communicate internally with other resources in the VPC.

### 3. Elastic IPs
An **Elastic IP** is a static, public IPv4 address that can be associated with an instance. Unlike public IPs, an Elastic IP remains the same even if the instance is stopped and started.

- **Use Case**: If you need a consistent public IP address for an instance, such as when hosting a website or service.

## Public Subnet in VPC
A **public subnet** is a subnet in a VPC that has access to the internet. Instances launched in a public subnet have public IP addresses, and they communicate externally using an **Internet Gateway (IGW)**.

### Deploying an EC2 Instance in a Public Subnet
When you deploy an EC2 instance in a public subnet, it is automatically assigned a public IP, enabling it to communicate directly with the internet through the Internet Gateway.

- **Internet Gateway**: This is a component that allows EC2 instances in public subnets to send and receive traffic from the internet.
- **External Access**: Since the instance has a public IP, it can be accessed directly from the internet (e.g., via SSH for Linux, RDP for Windows).

#### Key Steps:
1. Launch an EC2 instance in a public subnet.
2. Ensure the subnet has an associated route table that directs internet-bound traffic to the Internet Gateway.
3. Assign security groups to control inbound and outbound traffic (e.g., allowing SSH access on port 22).

## Private Subnet in VPC
A **private subnet** is designed for resources that do not need direct access to the internet. Instances in a private subnet do not receive public IPs and are more secure because they cannot be accessed from outside the VPC.

### Deploying an EC2 Instance in a Private Subnet
An EC2 instance in a private subnet does not have direct internet access. Instead, it can communicate with the internet using a **NAT Gateway** for outbound traffic, while still preventing any direct inbound connections from the internet.

- **NAT Gateway**: Allows instances in the private subnet to access the internet for updates or external resources but prevents external entities from initiating connections to the instance.
- **Inbound Traffic**: The instance only accepts inbound traffic that is routed through specific internal resources or security measures, ensuring enhanced security.

#### Key Steps:
1. Launch an EC2 instance in a private subnet.
2. Ensure the subnet uses a route table that sends internet-bound traffic to a NAT Gateway in a public subnet.
3. Control traffic using security groups and network ACLs, allowing internal communications while restricting external access.

### Summary of Differences
- **Public Subnet**: Uses an Internet Gateway, has external access, and is designed for instances that need to communicate with the internet directly.
- **Private Subnet**: Uses a NAT Gateway, does not have external access, and is used for instances that require a more secure, internal environment.

---
With these concepts, you now have a foundational understanding of how EC2 instances operate in public and private subnets, and how to manage IP addresses to control communication.
