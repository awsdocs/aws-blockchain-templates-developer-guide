# Getting Started with AWS Blockchain Templates<a name="blockchain-templates-getting-started"></a>

This tutorial demonstrates how to use the AWS Blockchain Template for Ethereum to create a private blockchain network on AWS through AWS CloudFormation\. The network that you create has two Ethereum clients and one miner running on Amazon EC2 instances in an Amazon ECS cluster\. Amazon ECS runs these services in Docker containers pulled from Amazon ECR\. Before you start this tutorial, it's helpful to know about blockchain networks and the AWS services involved, but not required\.

This tutorial assumes that you have set up the general prerequisites covered in [Setting Up AWS Blockchain Templates](blockchain-templates-setting-up.md)\. In addition, you must set up some AWS resources, such as an Amazon VPC network and specific permissions for IAM roles, before you use the template\.

The tutorial demonstrates how to set up those prerequisites\. We made setup choices, but they are not prescriptive\. As long as you meet the prerequisites, you can make other configuration choices based on the needs of your application and environment\. For information about the features and general prerequisites for each template, and to download templates or launch them directly in AWS CloudFormation, see [AWS Blockchain Templates and Features](blockchain-template-features.md)\.

Throughout this tutorial, examples use the US West \(Oregon\) Region \(us\-west\-2\), but you can use any region that supports AWS Blockchain Templates: 
+ US West \(Oregon\) Region \(us\-west\-2\)
+ US East \(N\. Virginia\) Region \(us\-east\-1\)
+ US East \(Ohio\) Region \(us\-east\-2\)

**Note**  
Running a template in a Region not listed above launches resources in the US East \(N\. Virginia\) Region \(us\-east\-1\)\.

The AWS Blockchain Template for Ethereum that you configure using this tutorial creates the following resources:
+ On\-Demand EC2 instances of the type and number that you specify\. The tutorial uses the default t2\.medium instance type\.
+ An internal Application Load Balancer\.

Following the tutorial, steps are provided to clean up resources that you create\.

**Topics**
+ [Set Up Prerequisites](blockchain-template-getting-started-prerequisites.md)
+ [Create the Ethereum Network](blockchain-templates-create-stack.md)
+ [Connect to EthStats and EthExplorer Using the Bastion Host](blockchain-bastion-host-connect.md)
+ [Clean Up Resources](blockchain-templates-cleanup.md)