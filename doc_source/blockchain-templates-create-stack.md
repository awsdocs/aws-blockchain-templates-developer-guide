# Create the Ethereum Network<a name="blockchain-templates-create-stack"></a>

The Ethereum network that you specify using the template in this topic launches an AWS CloudFormation stack that creates an Amazon ECS cluster of EC2 instances for the Ethereum network\. The template relies on the resources that you created earlier in [Set Up Prerequisites](blockchain-template-getting-started-prerequisites.md)\.

When you launch the AWS CloudFormation stack using the template, it creates nested stacks for some tasks\. After they are complete, you can connect to resources served by the network's Application Load Balancer through the bastion host to verify that your Ethereum network is running and accessible\.

**To create the Ethereum network using the AWS Blockchain Template for Ethereum**

1. Open the latest AWS Blockchain Template for Ethereum in the AWS CloudFormation console using the following quick\-links for the AWS Region in which you are working:
   + [Launch in US East \(N\. Virginia\) region \(us\-east\-1\)](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-blockchain-templates-us-east-1.s3.us-east-1.amazonaws.com/ethereum/templates/latest/ethereum-network.template.yaml)
   + [Launch in US East \(Ohio\) region \(us\-east\-2\)](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?templateURL=https://aws-blockchain-templates-us-east-2.s3.us-east-2.amazonaws.com/ethereum/templates/latest/ethereum-network.template.yaml)
   + [Launch in US West \(Oregon\) region \(us\-west\-2\)](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?templateURL=https://aws-blockchain-templates-us-west-2.s3.us-west-2.amazonaws.com/ethereum/templates/latest/ethereum-network.template.yaml)

1. Enter values according to the following guidelines:
   + For **Stack name**, enter a name that is easy for you to identify\. This name is used within the names of resources that the stack creates\.
   + Under **Ethereum Network Parameters** and **Private Ethereum Network Parameters**, leave the default settings\.
   + Under **Platform configuration**, leave the default settings, which creates an Amazon ECS cluster of EC2 instances\. The alternative, **docker\-local** creates an Ethereum network using a single EC2 instance\.
   + For **VPC ID**, select the VPC that you created earlier in [Create a VPC and Subnets](blockchain-template-getting-started-prerequisites.md#blockchain-templates-create-a-vpc)\.
   + For **List of VPC Subnets to use**, select the single private subnet that you created earlier in the procedure [To create the VPC](blockchain-template-getting-started-prerequisites.md#create-vpc-procedure)\.
   + For **Application Load Balancer Subnet IDs**, select two public subnets from the [list of subnets](blockchain-template-getting-started-prerequisites.md#list-of-subnets) that you noted earlier, after you created the VPC and the additional public subnet\.
   + For **EC2 Key Pair**, select a key pair\. For information about creating a key pair, see [Create a Key Pair](blockchain-templates-setting-up.md#blockchain-templates-create-a-key-pair)\.
   + For **EC2 Security Group**, select the security group you created earlier in [Create Security Groups](blockchain-template-getting-started-prerequisites.md#blockchain-templates-create-security-group)\.
   + For **IAM Role for ECS**, enter the ARN of the ECS role that you created earlier in [Create an IAM Role for Amazon ECS and an EC2 Instance Profile](blockchain-template-getting-started-prerequisites.md#blockchain-templates-iam-roles)\.
   + For **EC2 Instance Profile ARN**, enter the ARN of the instance profile that you created earlier in [Create an IAM Role for Amazon ECS and an EC2 Instance Profile](blockchain-template-getting-started-prerequisites.md#blockchain-templates-iam-roles)\.
   + For **Application Load Balancer Security Group**, select the security group for the Application Load Balancer that you created earlier in [Create Security Groups](blockchain-template-getting-started-prerequisites.md#blockchain-templates-create-security-group)\.
   + Under **ECS cluster configuration**, leave the defaults, which creates an ECS cluster of three EC2 instances\.\.
   + For **EthStats**, leave the default setting, which is *true*\.
   + For **EthStats Connection Secret**, type an arbitrary value that is at least six characters\.
   + For **EthExplorer**, leave the default setting, which is *true*\.

1. Leave all other settings to their defaults, select the acknowledgement check box, and choose **Create**\.

   The **Stack Detail** page for the root stack that AWS CloudFormation launches appears\.

1. To monitor the progress of the root stack and nested stacks, choose **Stacks**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/choose-stacks.png)

1. When all stacks show **CREATE\_COMPLETE** for **Status**, you can connect to Ethereum user interfaces to verify that the network is running and accessible\. When you use the ECS container platform, URLs for connecting to EthStats, EthExplorer, and EthJsonRPC through the Application Load Balancer are available on the **Outputs** tab of the root stack\.
**Important**  
You won't be able to connect directly to these URLs or SSH directly until you set up a proxy connection through the bastion host on your client computer\. For more information, see [Connect to EthStats and EthExplorer Using the Bastion Host](blockchain-bastion-host-connect.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/stack-urls.png)