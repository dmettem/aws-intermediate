footer: © NodeProgram.com, Node.University and Azat Mardan 2017
slidenumbers: true
theme: Merriweather, 8

# AWS Intermediate
## Lab

![inline 100%](images/azat.jpeg)
Azat Mardan @azat_co

![inline right](images/nu.png)

[.slidenumbers: false]

---

# Overview

1. DevOps and Infrastructure as code
1. Identity Access Management in AWS
1. Working with AWS CLI
1. Cloud Infrastructure Automation with CloudFormation
1. Building CI/CD with GitHub, CodeDeploy and CodePipeline

---

# Overview (cont.)

1. SDK and Node SDK Example
1. Working with ElasticBeanstalk
1. Working with Docker: EC2, ECS and Elastic Beanstalk Containers
1. Connecting Resources and IAM, Extra Services and Best Practices
1. Summary and further study

---

## We have a few pre-requisites—tools you need to install before we can proceed. But where to install?


---

# Install Pre-Reqs Here:

* Host - your dev machine (recommended for Mac and Linux)
* Virtual machine - if you develop in VM (recommended for Windows)
* Remote machine - if you develop in the cloud or if you are setting up CD/CI environment

---

# Pre-Reqs

* AWS Account (requires email + credit/debit card)
* Python 2.7 or 3.x (latest is better)
* [AWS CLI](https://aws.amazon.com/cli): Install with [pip](https://pypi.python.org/pypi/pip) or brew or just use a [bundle](http://docs.aws.amazon.com/cli/latest/userguide/awscli-install-bundle.html) (see all [options](http://docs.aws.amazon.com/cli/latest/userguide/installing.html))
* Node and npm for HTTP server, tools and SKD code ([installers](https://nodejs.org/en/download))
* [Docker](https://www.docker.com) deamon/engine - advanced if we have time ([instructions](https://docs.docker.com/engine/installation))

---

Good to have tools

* [Git](https://git-scm.com) mostly for code deploys and Elastic Beanstalk
* Code editor [Atom](https://atom.io) or [VS code](https://code.visualstudio.com)
* [CURL](https://curl.haxx.se/download.html) and [PuTTY](http://www.putty.org) (for Windows)

---

# DevOps and Infrastructure as code

---

# What is DevOps

* Speed of delivery - business value
* Reduce errors (automate)
* Save cost
* Bridge gap between IT Ops and devs - work as one team

---

# Main Principles

* Automate everything
* Version: define infrastructure as code and store in version control system (like app code)
* Ability to deploy and roll back quickly and *often*
* Test app and infrastructure code

---

# Continuous Integration (CI)

For apps:

Dev -> Code version control repository -> Build and Test on CI server -> Deploy to QA -> deploy to prod

For infra:

IT Ops -> Repo -> CI Server: Build images and validate templates (CloudFormation), test APIs -> Deploy

---

# Continuous Delivery (CD)

CI/CD pipeline is automation of CIs. Could include stress testing and performance testing.

Not the same as Continuous Deployment (delivery has manual prod deploy).

---

# Infrastructure as Code

* Repeatability: Humans make mistakes, machines less so (almost 0 when hardware is robust)
* Agility: Deploy quickly and often and roll back quickly and predictably if needed
* Auditing: Permissions and ACL with a history

---

# Cloud Automation

* AWS CLI
* SDKs
* CloudFormation
* Others: Ansible, Terraform

---

# What about AWS and its tools?

---

# AWS DevOps and Automation

* Provision environment/infrastructure: AWS CLI, CloudFormation, OpsWorks, Beanstalk
* Configuring servers with AWS: User Data, Docker, Beanstalk, CodeDeploy
* Configuring servers with other tools: Chef, Puppet SaltStack, Ansible

---

# What we need to do for CI

1. Provision environment
1. Deploy code
1. Build
1. Test
1. Verify

---

# AWS CLI

---

## AWS CLI is a very basic way to automate infrastructure and save it in code.

---

## AWS CLI Benefits

* Infrastructure as code: can save in a file and version
* Repeatability: bash script can be run multiple times
* Error free: no need to remember all the steps and configuration for web console
* Fast: no need to click around in the web console
* Can be run from any machine: Will work for CI/CD

---

## AWS CLI Installation

```
phyton --version
pip --version
pip install awscli
```

At least 2.6.5 or 3.x (recommended)

<http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html>


---

## Other AWS CLI Installations

* [Install the AWS CLI with Homebrew](http://docs.aws.amazon.com/cli/latest/userguide/cli-install-macos.html#awscli-install-osx-homebrew) - for macOS
* [Install the AWS CLI Using the Bundled Installer (Linux, macOS, or Unix)](http://docs.aws.amazon.com/cli/latest/userguide/awscli-install-bundle.html) - just download, unzip and execute

---

## AWS CLI Usage Pattern

```
aws <command> <subcommand> [options and parameters]
```

---

## Auth with AWS

* Access Key ID
* Secret Access Key

Copy your key and secret (root) or create a new user, give appropriate permissions and copy key and secret for that user (best practice).

Note: You can use AWS CLI to create a user too.

---

![fit](images/aws-security-credentials.png)

---

![fit](images/aws-security-credentials-2.png)

---

![fit](images/aws-security-credentials-3.png)

---

![fit](images/aws-security-credentials-4.png)


---

## Configure Your CLI

```
aws configure
```

1. Provide access key ID
1. Provide secret access key
1. Set region to `us-west-1` and output to None or json


---

## Example

```
aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-1
Default output format [None]: json
```

---

# Getting Help

```
aws help
aws ec2 help
aws ec2 describe-regions help
```

---

# Create User with CLI—Easy!

Create user:

```
aws iam create-user --user-name MyUser
```

Attache policy from a file:

```
aws iam put-user-policy --user-name MyUser --policy-name MyPowerUserRole --policy-document file://C:\Temp\MyPolicyFile.json
```

Or a link:

```
aws iam put-user-policy --user-name MyUser --policy-name MyPowerUserRole --policy-document https://s3.amazonaws.com/checkr3/CC_IAM_FullPolicy.json
```

---

## IAM Policy Example for EC2 Allow All:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1491182980154",
      "Action": "ec2:*",
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

Another [IAM JSON file example](https://s3.amazonaws.com/checkr3/CC_IAM_FullPolicy.json)

---

# Documentation on IAM

* [AWS Identity and Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html)
* [AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)

---

List policies for the user to verify:

```
aws iam list-user-policies --user-name MyUser
```


Create password to login to web console:

```
aws iam create-login-profile --user-name MyUser --password Welc0m3!
```

Create access key:

```bash
aws iam create-access-key --user-name MyUser
```

---

## Create access key response example:

```json
{
    "AccessKey": {
        "UserName": "Bob",
        "Status": "Active",
        "CreateDate": "2015-03-09T18:39:23.411Z",
        "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        "AccessKeyId": "AKIAIOSFODNN7EXAMPLE"
    }
}
```

---

# Getting Started with AWS CLI

```
aws ec2 describe-instances help
aws ec2 run-instances help
awc ec2 create-images help
aws ec2 describe-images help

```

---

# Launch Instance

1. Get image ID
1. Run command

---

## Getting Image ID

* Web console
* `describe-images`

---

## Example of Amazon Linux AMI 2016.09.1 (HVM), SSD Volume Type


```
aws ec2 describe-images --owners amazon \
  --filters "Name=virtualization-type,Values=hvm" "Name=root-device-type,Values=ebs" \
  "Name=name,Values=amzn-ami-hvm-2016.09.1.20170119-x86_64-gp2"
```

Result is to get "ami-165a0876"

---

Example of Amazon Linux AMI 2016.09.1 (HVM), SSD Volume Type Output:

```json
{
    "Images": [
        {
            "VirtualizationType": "hvm",
            "Name": "amzn-ami-hvm-2016.09.1.20170119-x86_64-gp2",
            "Hypervisor": "xen",
            "ImageOwnerAlias": "amazon",
            "EnaSupport": true,
            "SriovNetSupport": "simple",
            "ImageId": "ami-165a0876",
            "State": "available",
            "BlockDeviceMappings": [
                {
                    "DeviceName": "/dev/xvda",
                    "Ebs": {
                      ...
                    }
                }
            ],
            "Architecture": "x86_64",
            "ImageLocation": "amazon/amzn-ami-hvm-2016.09.1.20170119-x86_64-gp2",
            "RootDeviceType": "ebs",
            "OwnerId": "137112412989",
            "RootDeviceName": "/dev/xvda",
            "CreationDate": "2017-01-20T23:39:56.000Z",
            "Public": true,
            "ImageType": "machine",
            "Description": "Amazon Linux AMI 2016.09.1.20170119 x86_64 HVM GP2"
        }
    ]
}
```

---

# Run instances is really launch instances (or create)

```
$ aws ec2 run-instances --image-id ami-xxxxxxxx \
  --count 1 --instance-type t2.micro \
  --key-name MyKeyPair --security-groups my-sg
```

Note: Need to have security group first (if you don't have it).

---


# Run (Launch) instances with subnet:

```
$ aws ec2 run-instances --image-id ami-{xxxxxxxx} \
  --count 1 --instance-type t2.micro \
  --key-name {MyKeyPair} \
  --security-group-ids sg-{xxxxxxxx} --subnet-id subnet-{xxxxxxxx}
```

Note: Need to have security group and subnet first (if you don't have them).


---

# Working with Security Groups Example

```
aws ec2 create-security-group --group-name MySecurityGroup --description "My security group"
```

Add RDP port 3389:

```
aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 3389 --cidr 203.0.113.0/24
```

Add SSH port 22:
```
aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 22 --cidr 203.0.113.0/24
```

Verify security group:

```
aws ec2 describe-security-groups --group-names my-sg
```

---

## Security Group Open Everything Example

```
aws ec2 create-security-group --group-name open-sg --description "Open security group"
aws ec2 authorize-security-group-ingress --group-name open-sg --protocol all --port 0-65535 --cidr 0.0.0.0/0
aws ec2 describe-security-groups --group-names open-sg
```

---

## Adding Tags

```
aws ec2 create-tags --resources i-{xxxxxxxx} \
  --tags Key={Name},Value={MyInstance}
```

Replace {xxx}, {Name} and {MyInstance}

---

```
aws ec2 describe-instances
```

---

## Stopping, starting and terminating

```
aws ec2 stop-instances --instance-ids i-{xxxxxxxx}
aws ec2 start-instances --instance-ids i-{xxxxxxxx}
aws ec2 terminate-instances --instance-ids i-{xxxxxxxx}
```

Note: after stop you can start, after terminate no.

---

# Working with Key Pairs

```
aws ec2 create-key-pair --key-name {MyKeyPair} \
  --query 'KeyMaterial' --output text > {MyKeyPair}.pem
aws ec2 describe-key-pairs --key-name {MyKeyPair}
aws ec2 delete-key-pair --key-name {MyKeyPair}
```

---


# Auto Startup

* `init.d` or [CloudInit](https://help.ubuntu.com/community/CloudInit) for Ubuntu+Debian and other like CentOS with additional [installation](http://stackoverflow.com/questions/23411408/how-do-i-set-up-cloud-init-on-custom-amis-in-aws-centos/23411409#23411409)
* User Data
* Command

---

```sh
#!/bin/bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 6
node -e "console.log('Running Node.js ' + process.version)"
echo "require('http').createServer((req, res) => {
  res.end('hello world')
}).listen(3000, (error)=>{
  console.log('server is running on 3000')
})
" >> index.js
node index.js
```

---

# Shell Script and User Data Example

```sh
#!/bin/bash
yum update -y
yum install -y httpd24 php56 mysql55-server php56-mysqlnd
service httpd start
chkconfig httpd on
groupadd www
usermod -a -G www ec2-user
chown -R root:www /var/www
chmod 2775 /var/www
find /var/www -type d -exec chmod 2775 {} +
find /var/www -type f -exec chmod 0664 {} +
echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
```

---


More info on User Data:

<http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html>

---

# Deploying Code

---


---

# Source Code

* Git - `git push origin master` and then `git pull origin master`
* Rsync `rsync -avzhe ssh backup.tar ec2-user@192.168.0.100:/backups/`
* S3, e.g., `aws s3 cp s3://{mybucket}/latest/install . --region us-east-1` and then curl or wget

---


# CodeDeploy

* <https://aws.amazon.com/codedeploy>

---

1. Create role CDInstanceRole in IAM (AmazonEC2RoleforAWSCodeDeploy)
1. Create role CDServiceRole (AWSCodeDeployRole)

Use CLI or web console


---

AmazonEC2RoleforAWSCodeDeploy Policy in JSON (for CLI)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListObjects"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```


---

Policy for AWSCodeDeployRole in JSON (for CLI)

![inline](images/codedeployrole.png)

---

# 2. Create an instance

* Amazon Linux t2.micro
* use CDInstanceRole in IAM role so EC2 instance
* Install codedeploy agent in User Data (code/install-codedeploy-agent.sh)
* Use 8Gb SSD
* Tag with env=dev
* Add SSH and HTTP for security group

---

## code/install-codedeploy-agent.sh

```sh
#!/bin/bash
yum install -y aws-cli
cd /home/ec2-user/
aws s3 cp 's3://aws-codedeploy-us-east-1/latest/codedeploy-agent.noarch.rpm' . \
  --region us-east-1
yum -y install codedeploy-agent.noarch.rpm
```

---

# Create custom CodeDeploy deployment

* Use NodeApp and NodeAppInstances for app name and deployment group name
* Use tag env=dev
* Deployment config = All at once
* Use Service Role ARN = CDServiceRole

---

# Deploy options

* From S3 (using CLI)
* From GitHub (AWS CodePipeline)

---

# Create CodePipeline

* Enter name
* Select GitHub as provider and select "Connect to GitHub"
* Pick repository
* Skip the build
* Beta: AWS CodeDeploy, use NodeApp and NodeAppInstances
* Create pipeline service role using wizard (or manually)

---

# Verifying

1. Observe the code deploy in the pipeline (Source and Beta stages)
1. Copy public URL of one of your instances and navigate to it in the browser
1. Edit GitHub source (index.js)
1. See changes

---

# Other options for CI/CD (might be complimentary to CodeDeploy)

* Jenkins
* TravisCI
* Bamboo
* TeamCity
* CircleCI
* CruiseControl

---

# Pipiline

---

# CloudFormation

[Samples](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-us-west-2.html)

---

# OpsWork vs CloudFormation vs Elastic Beanstalk

OpsWork: configuration management (stacks and layers) - narrower app-oriented resources than CloudFormation

CloudFormation: building block service for almost everything

Elastic Beanstalk: only app management service

---

# Lab

Goal: Use CloudFormation to create [Autoscaling and load-balancing website in an Amazon VPC](https://s3-us-west-2.amazonaws.com/cloudformation-templates-us-west-2/VPC_AutoScaling_and_ElasticLoadBalancer.template)

---


# Connecting Resources and IAM

---

# Best IAM Practices

* Lock away your AWS account (root) access keys
* Create individual IAM users
* Use AWS-defined policies to assign permissions whenever possible
* Use groups to assign permissions to IAM users
* Grant least privilege

---

# Best IAM Practices (Cont)

* Configure a strong password policy for your users
* Enable MFA for privileged users
* Use roles for applications that run on Amazon EC2 instances
* Delegate by using roles instead of by sharing credentials


---

# Best IAM Practices (Cont)

* Rotate credentials regularly
* Remove unnecessary credentials
* Use policy conditions for extra security
* Monitor activity in your AWS account

---


# How to access and work with AWS platform from within your application?

---

# SDKs!

---

Supported

* Amazon S3
* Amazon EC2
* DynamoDB
* Many more!

---

# Node SDK

```
mkdir aws-node-sdk-test
cd aws-node-sdk-test
npm init -y
npm i -SE aws-sdk
```

---

# Credentials

* Home directory
* Environment variables
* JavaScript/Node or JSON file

---

# Credentials in Home Directory

 `~/.aws/credentials` or `C:\Users\USER_NAME\.aws\credentials` for Windows users

```
[default]
aws_access_key_id = YOUR_ACCESS_KEY_ID
aws_secret_access_key = YOUR_SECRET_ACCESS_KEY
```

---

## EC2 Example

![inline](images/code-samples-ec2.png)

---

Create and open `create-ec2.js`:

```js
// Load the SDK for JavaScript
var AWS = require('aws-sdk');

// Load credentials and set region from JSON file
AWS.config.loadFromPath('./config.json');
```

---

```js
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set region from JSON file
AWS.config.loadFromPath('./config.json');

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
   ImageId: 'ami-10fd7020', // amzn-ami-2011.09.1.x86_64-ebs
   InstanceType: 't2.micro',
   MinCount: 1,
   MaxCount: 1
};
```

---

```js
// Create the instance
ec2.runInstances(params, function(err, data) {
   if (err) {
      console.log("Could not create instance", err);
      return;
   }
   var instanceId = data.Instances[0].InstanceId;
   console.log("Created instance", instanceId);
   // Add tags to the instance
   params = {Resources: [instanceId], Tags: [
      {
         Key: 'Name',
         Value: 'SDK Sample'
      }
   ]};
   ec2.createTags(params, function(err) {
      console.log("Tagging instance", err ? "failure" : "success");
   });
});
```

---

# Lab

## Goal: Build automation with User Data and AWS CLI

---

* Create a shell script (script A) to install and run Node.js on Amazon Linux:
  * Install EPEL and Node (npm included) [link](https://gist.github.com/azat-co/06bb3505185c8f5aa3d2ca61f9e50098)
  * Install pm2: `npm i pm2 -g`
  * Download server code from [GitHub](https://gist.githubusercontent.com/azat-co/8f3eb8461c3179c9616cd45b99e1d1b8/raw/00774756f9bce388f9f0614b2bf2c8528b745434/server.js) using wget or curl ([link](http://stackoverflow.com/questions/14300794/how-do-i-download-a-file-from-the-internet-to-my-linux-server-with-bash)
  * Start server with `sudo pm2 start app.js -i 0`
* Use script A in User Data for `run-instances`
* Create a shell script (B) to create a 2 new instance `run-instances` from Amazon Linux in region `us-west-2` with script A on start.

---

# Lab 1 Cont.

1. Test by going to the browser
1. Create autoscaling group: CPU>10% +1 (aws cli)
1. Install loadtest: `npm i -g loadtest`
1. Use loadtest to stress test your instance to see if autoscaling kicks in.
1. Terminate the instance(s) with aws cli


---

# Lab 2

Use Elastic Beanstalk to deploy a web app which uses RDS:

<http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_nodejs.html>

---

# Lab 3

Set up CloudFormation for the stack:

* ELB
* 2 EC2 with a web app which uses DynamoDB
* Security Group
* VPC
* DynamoDB
* SNS

---

# Resources for Lab 3:

* <http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/CHAP_TemplateQuickRef.html>

* <http://docs.aws.amazon.com/amazondynamodb/latest/gettingstartedguide/GettingStarted.NodeJs.html>
* <http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-examples.html>
