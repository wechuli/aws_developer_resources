# Building on AWS

## Infrsatructure

- **Availability Zone** - Logical Datacenter where you run your code e.g us-west-2b -end in letters
- **Region** - e.g us-west-2 region has AZs which are separated from each other in the event of a catastrophe.

AZ scope -EC2
Region Scope -S3

## Networking in AWS

**VPC** - Virtual Private Cloud
You can define subnets on the VPC. Resources inside a subnets can communicate with each other
A VPC has region scope, Subnets are AZ scope

## EC2 Metadata

    $ curl http://169.254.169.254/latest/dynamic/instance-identity/document

## Key Topics

### AWS Cloud

The AWS Cloud lets you build applications quickly and cost effectively - you pay for the resources you need and can quickly add more resources when you need them.

### Free Tier

You can explore AWS and complete the exercises for this course within the AWS Free Tier. AWS automatically provides alerts using AWS Budgets to help you track your free tier usage. See AWS Free Tier Usage Alerts using AWS Budgets for more.

### EC2

Amazon Elastic Compute Cloud allows you to run virtual servers in AWS.
Your virtual server is known as an EC2 Instance. It runs on a physical host that is inside an AWS Availability Zone (AZ). There will be 2 or more AZ within an AWS Region. This design allows you to build applications that are resilient to large scale events that could impact an AZ.

If you'd like to learn more about AWS facilities, take a 'digital tour' of an AWS data center!

### VPC

Your network in AWS is provided by Amazon Virtual Private Cloud (VPC). You can create a VPC within an AWS region and within that VPC you define subnets to manage related sets of servers or other AWS resources. VPC lets you define rules for how network traffic from your subnets is routed. You can also decide whether your network should be connected to the Internet, to corporate networks, or to keep the network completely private.

The IP Address ranges for VPC and Subnets are specified using CIDR notation. If you'd like to know more about IP addressing within VPC, see the VPCs and Subnets in the User Guide for Amazon VPC. You can also learn more about CIDR notation in section 3.1 of RFC4632 or in Classless Interdomain Routing on Wikipedia.

### Security in AWS

You are given a lot of flexibility in AWS to configure and build your applications the way you want. Given that you control your resources, security in AWS is a shared responsibility between AWS and you. AWS will provide secure facilities and building blocks for your application. AWS also provides guidance, and tools that can help you operate securely.

For example, if you are using EC2, it is your responsibility to take advantage of features such as Security Groups (firewall), Private Subnets (to provide network isolation) and encryption options to build secure applications. You are also responsible for keeping the operating system and application stack patched on your server.

If you use AWS managed services like RDS, you still have to make security decisions, but operational tasks like patching the Operating System and SQL engine can be done automatically on your behalf. When using APIs like Amazon S3 API, the underlying infrastructure and maintenance is fully abstracted from you and you are only responsible for calling the API and configuring your access and encryption policies.

For more on the Shared Security Model, see Shared Responsibility Model on the AWS Compliance site.

Additional Services Used

### CloudFormation

An AWS service that can take in a declarative document called a 'template' and use it to provision AWS resources on your behalf so you don't have to. We used this to create a VPC to the specifications needed for the course.

### EC2 Metadata service

This is a service that intercepts calls to 169.254.169.254 from your EC2 instance to communicate metadata to the instance. This IP address is in the range for IPv4 Link-Local IP addresses as defined by RFC 3927 and the details about the properties the instance that can be retrieved are documented here in the EC2 User Guide.

What you accomplished this week
You signed up for an AWS Account
You launched your first web server into AWS
You built the virtual network we'll use in upcoming exercises and connected to your EC2 instance

AWS-ACCESS_KEY
AWS_SECRET_KEY

IAM- Identity and Access Management - Access control

You should not use your AWS account root user credentials to access AWS. Instead, create an AWS IAM user and assign permissions only necessary for the work done by the user.

An AWS IAM user is an entity that you create in AWS to represent the person or service that uses it to interact with AWS. You attach permission policies to the IAM user that determines what the user can and cannot do in AWS. Access keys are a combination of an access key ID and a secret access key that are assigned to a user. These can be used to make programmatic calls to AWS when using the API in program code or at a command prompt when using the AWS CLI.

When you create IAM policies, follow the standard security advice of granting least privilege - that is, grannting only the permissions required to perform a task. Determine what users need to do and then craft policies for them that let the users perform only those tasks.

Boto 3 is the AWS SDK for python , making it easier to integrate your Python application, library, or script with AWS services.

## S3

Amazon S3 or Amazon Simple Storage Service is a service offered by AWS that provides object storage through a web service interface.

## Application Architecture

![](architecture.jpg)

The application is deployed on an Amazon EC2 instance with an Application Load Balancer sitting in front of the instance to direct user requests to the instance. Amazon Cognito is used to sign up/sign in users for the application. In order to asynchronously process the photo labels, when a photo is uploaded, an Amazon S3 bucket event notification is issued to an Amazon SNS topic. This triggers a subscribed AWS Lambda function, which talks to Amazon Rekognition. To make the application more distributed, an Amazon SQS queue subscribed to the Amazon SNS topic stores all the incoming requests and an on-premises application polls the queue for processing. AWS X-Ray traces the calls made to all the AWS resources in this application, thereby providing diagnostics information. The application is coded in Python 3 using AWS Cloud9 as the IDE.

### AWS SDKs

AWS SDKs are available for many popular programming languages. The AWS SDK for Python, is called Boto3.

If you would like to install and configure Boto3 locally, review the the Boto3 Quickstart on the Read The Docs site.

SDK documentation will include both high level guides and reference documentation. You can find the documentation for Boto3 at Read the Docs.

### AWS Identity and Access Management

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. We typically use credentials from IAM Users or IAM Roles to authenticate with AWS when making API calls. We control the permissions for which API actions those Users or Roles can perform with IAM Policies.

### Making AWS API Calls

AWS API requests are made against a specific API endpoints located in a specific AWS Region. In this class, we are using the "us-west-2" region which is located in Oregon. For example, here are the API endpoints for Rekognition and the API endpoints for S3.

API requests are typically signed with an access key belonging to an IAM User and using the Signature Version 4 Signing Process. It is also possible to sign these API requests using temporary security credentials such as those derived from an IAM Role.

AWS SDKs check several locations for credentials such as local configuration files and environment variables. If the SDK finds credentials, it will automatically sign your API requests for you. For example, here is how the Python SDK checks for Credentials.

### Developing in the cloud with AWS Cloud9

AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. You can run this development environment on a managed Amazon EC2 instance that automatically sleeps when you aren't using it.

Make sure to follow the exercise directions carefully when setting up your Cloud9 instance - it needs to be launched in the specified VPC so that you can access resources that you'll be creating in the coming weeks.

### Amazon S3

Amazon Simple Storage Service (S3) is object storage built to store and retrieve any amount of data from anywhere – web sites and mobile apps, corporate applications, and data from IoT sensors or devices. You can store files as Objects within S3 Buckets.

In this course, we are using Amazon S3 to store photos.

By default, the objects you put into S3 are private. S3 allows you to use Bucket Policies, IAM Policies, and ACLs to grant permissions to the contents of the bucket. You can also use Presigned URLs for time-limited access to objects.

Presigned URLs are how we are granting access to images in our Python Application. The S3 Service Feature Guide for Boto3 contains Python examples of working with S3. In particular, you should review the sectin on Generating Presigned Urls.

### Amazon Rekognition

Amazon Rekognition is a service that applies deep learning to analyze the contents of images and videos. It supports functionality such scene detection, face detection, even celebrity recognition. In this class, we are using the Detect Labels functionality which takes an image as input and returns labels with confidence values such as {Name: lighthouse, Confidence: 98.4629}

### More on AWS Identity And Access Management

When you log in to AWS using your email address and password, you are authenticating as the Root User for the account. The best practice is to avoid logging in as Root except for a handful of operations that only the root user can perform.

Instead, you can create IAM Users within your AWS Account for yourself and for any others that need access to resources in your account. IAM Users have a set of permanent credentials such as an Access Key for API access or a Console Password.

Permissions are granted or denied with IAM Policies. By default, all permissions are denied unless explicitly granted. You may select predefined permissions from the list of AWS Managed Policies or define your own custom IAM policies.

If you find that you'll have several users who need similar permissions, you can define an IAM Group and associate your users to the group.

When working with AWS Services, you may also encounter IAM Roles. Many AWS services require that you use roles to control what that service can access. IAM Roles provide only temporary security credentials. One common case is allowing the EC2 service to distribute credentials to your application code running on an EC2 instance. Roles can also enable other scenarios in the enterprise such as cross-account access and identity federation.

### AWS Signature V4 Process

If you'd like to explore Signature V4 Signing further, here's some example code that creates HTTP requests and generates the appropriate signature.

What you accomplished this week
You created an IAM User and are following the best practice of not using Root User in your AWS Account
You installed the AWS SDK and configured credentials
You launched and configured Cloud9 IDE environment
You ran a Python Application that makes AWS API calls to Amazon S3 and AWS Rekognition
