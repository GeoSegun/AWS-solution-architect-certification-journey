# Install tools and configure AWS CLI

The tools necessary to get started are:

1. **VS code:** To download VS code, you'll headover to google and search for "Vs code Download". Follow the [link](https://code.visualstudio.com/download) and install based on your operating system.

2. **AWS CLI:** This is a command line interface that allows you to interract with your AWS resources programmatically. You need to follow these [link]() to install based on your operating system.

To install AWS CLI on Linux, we simply run the following command:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

To check the version of AWS cli installed. You simply run the below command:

```bash
aws --version
```

3. **Access AWS cloudshell:** AWS cloud shell is a commandline interface in your AWS account that allows you run command line task in the cloud without installing or configuring any software locally

N/B: To check if you've successfully connected to your AWS console programmatically, you use the below command:

```bash
aws sts get-caller-identity
```
