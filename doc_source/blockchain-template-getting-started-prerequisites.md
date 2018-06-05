# Set Up Prerequisites<a name="blockchain-template-getting-started-prerequisites"></a>

The AWS Blockchain Template for Ethereum configuration that you specify in this tutorial requires that you do the following:
+ [Create a VPC and Subnets](#blockchain-templates-create-a-vpc)
+ [Create Security Groups](#blockchain-templates-create-security-group)
+ [Create an IAM Role for Amazon ECS and an EC2 Instance Profile](#blockchain-templates-iam-roles)
+ [Create a Bastion Host](#blockchain-templates-bastion-host)

## Create a VPC and Subnets<a name="blockchain-templates-create-a-vpc"></a>

The AWS Blockchain Template for Ethereum launches resources into a virtual network that you define using Amazon Virtual Private Cloud \(Amazon VPC\)\. The configuration you specify in this tutorial creates an Application Load Balancer, which requires two public subnets in different Availability Zones\. In addition, a private subnet is required for the container instances, and the subnet must be in the same Availability Zone as the Application Load Balancer\. You first use the VPC Wizard to create one public subnet and a private subnet in the same Availability Zone\. You then create a second public subnet within this VPC in a different Availability Zone\.

For more information, see [What is Amazon VPC?](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/) in the *Amazon VPC User Guide*\.

Use the Amazon VPC console \([https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\) to create the Elastic IP address, the VPC, and the subnet as described below\.

**To create an Elastic IP address**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Choose **Elastic IPs**, **Allocate new address**, **Allocate**\.

1. Make a note of the Elastic IP address that you create and choose **Close**\.

1. In the list of Elastic IP addresses, find the **Allocation ID** for the Elastic IP address created earlier\. You use this when you create the VPC\.

**To create the VPC**

1. From the navigation bar, select a Region for the VPC\. VPCs are specific to a Region, so select the same Region in which you created your key pair in and where you are launching the Ethereum stack\. For more information, see [Create a Key Pair](blockchain-templates-setting-up.md#blockchain-templates-create-a-key-pair)\.

1. On the VPC dashboard, choose **Start VPC Wizard**\.

1. On the **Step 1: Select a VPC Configuration** page, choose **VPC with Public and Private Subnets**, **Select**\.

1. On the **Step 2: VPC with Public and Private Subnets ** page, leave **IPv4 CIDR block** and **IPv6 CIDR block** to their default values\. For **VPC name**, enter a friendly name\.

1. For **Public subnet's IPv4 CIDR**, leave the default value\. For **Availability Zone**, choose a zone\. For **Public subnet name**, enter a friendly name\.

   You specify this subnet as one of the first of two subnets for the Application Load Balancer when you use the template\.

   Note the Availability Zone of this subnet because you select the same Availability Zone for the private subnet, and a different one for the other public subnet\.

1. For **Private subnet's IPv4 CIDR**, leave the default value\. For **Availability Zone**, select the same Availability Zone as in the previous step\. For **Private subnet name**, enter a friendly name\.

1. For **Elastic IP Allocation ID**, select the Elastic IP address that you created earlier\.

1. Leave the default values for other settings\.

1. Choose **Create VPC**\.

   The example below shows a VPC **EthereumNetworkVPC** with a public subnet **EthereumPubSub1** and a private subnet **EthereumPvtSub1**\. The public subnet uses Availability Zone **us\-west\-2a**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/VPC.png)

**To create the second public subnet in a different Availability Zone**

1. Choose **Subnets** and then select the public subnet that you created earlier from the list\. Select the **Route Table** tab and note the **Route table** ID\. You specify this same route table for the second public subnet below\.

1. Choose **Create Subnet**\.

1. For **Name tag**, enter a name for the subnet\. You use this name later when you create the bastion host in this network\.

1. For **VPC**, select the VPC that you created earlier\.

1. For **Availability Zone**, select a different zone from the zone that you selected for the first public subnet\.

1. For **IPv4 CIDR block**, enter **10\.0\.2\.0/24**\.

1. Choose **Yes, Create**\. The subnet is added to the list of subnets\.

1. With the subnet selected from the list, choose **Subnet Actions**, **Modify auto\-assign IP settings**\. Select **Auto\-assign IPs**, **Save**, **Close**\. This allows the bastion host to obtain a public IP address when you create it in this subnet\.

1. On the **Route Table** tab, choose **Edit**\. For **Change to**, select the route table ID that you noted earlier and choose **Save**\.

You should now see three subnets for the VPC that you created earlier\. Make a note of the subnet names and IDs so that you can specify them using the template\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/subnets-listing.png)

## Create Security Groups<a name="blockchain-templates-create-security-group"></a>

Security groups act as firewalls, controlling inbound and outbound traffic to resources\. When you use the template to create an Ethererum network on an Amazon ECS cluster, you specify two security groups:
+ A security group for EC2 instances that controls traffic to and from EC2 instances in the cluster
+ A security group for the Application Load Balancer that controls traffic between the Application Load Balancer, EC2 instances, and the bastion host\. You associate this security group with the bastion host as well\.

Each security group has rules that allow communication between the Application Load Balancer and the EC2 instances, as well as other minimum rules\. This requires that the security groups reference one another\. For this reason, you first create the security groups and then update them with appropriate rules\.

**To create two security groups**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Security Groups**, **Create Security Group**\.

1. For **Security group name**, enter a name for the security group that's easy to identify and will differentiate it from the other, such as *EthereumEC2\-SG* or *EthereumALB\-SG*\. You use these names later\. For **Description**, enter a brief summary\.

1. For **VPC**, select the VPC that you created earlier\.

1. Choose **Create**\.

1. Repeat the steps above to create the other security group\.

**Add inbound rules to the security group for EC2 instances**

1. Select the security group for EC2 instances that you created earlier

1. On the **Inbound** tab, choose **Edit**\.

1. For **Type**, choose **All traffic**\. For **Source**, leave **Custom** selected, and then choose the security group you are currently editing from the list, for example, *EthereumEC2\-SG*\. This allows the EC2 instances in the security group to communicate with one another\.

1. Choose **Add Rule**\.

1. For **Type**, choose **All traffic**\. For **Source**, leave **Custom** selected, and then choose the security group for the Application Load Balancer from the list, for example, *EthereumALB\-SG*\. This allows the EC2 instances in the security group to communicate with the Application Load Balancer\.

1. Choose **Save**\.

**Add inbound and edit outbound rules for the security group for the Application Load Balancer**

1. Select the security group for Application Load Balancers that you created earlier

1. On the **Inbound** tab, choose **Edit** and then add the following inbound rules:

   1. For **Type**, choose **All traffic**\. For **Source**, leave **Custom** selected, and then choose the security group you are currently editing from the list, for example, *EthereumALB\-SG*\. This allows the Application Load Balancer to communicate with itself and with the bastion host\.

   1. Choose **Add Rule**\.

   1. For **Type**, choose **All traffic**\. For **Source**, leave **Custom** selected, and then choose the security group for EC2 instances from the list, for example, *EthereumEC2\-SG*\. This allows the EC2 instances in the security group to communicate with the Application Load Balancer and the bastion host\.

   1. Choose **Add Rule**\.

   1. For **Type**, choose **SSH**\. For **Source**, select **My IP**, which detects your computer's IP CIDR and enters it\.
**Important**  
This rule allows the bastion host to accept SSH traffic from your computer, enabling your computer to use the bastion host to view web interfaces and connect to EC2 instances on the Ethereum network\. To allow others to connect to the Ethereum network, add them as sources to this rule\. Only allow inbound traffic to trusted sources\.

   1. Choose **Save**\.

1. On the **Outbound** tab, choose **Edit** and delete the rule that was automatically created to allow outbound traffic to all IP addresses\.

1. Choose **Add Rule**\.

1. For **Type**, choose **All traffic**\. For **Source**, leave **Custom** selected, and then choose the security group for EC2 instances from the list\. This allows outbound connections from the Application Load Balancer and the bastion host to EC2 instances in the Ethereum network\.

1. Choose **Add Rule**\.

1. For **Type**, choose **All traffic**\. For **Source**, leave **Custom** selected, and then choose the security group you are currently editing from the list, for example, *EthereumALB\-SG*\. This allows the Application Load Balancer to communicate with itself and with the bastion host\.

1. Choose **Save**\.

## Create an IAM Role for Amazon ECS and an EC2 Instance Profile<a name="blockchain-templates-iam-roles"></a>

When you use this template, you specify an IAM role for Amazon ECS and an EC2 instance profile\. The permissions policies attached to these roles allow the AWS resources and instances in your cluster interact with other AWS resources\. For more information, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\. You set up the IAM role for Amazon ECS and the EC2 instance profile using the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\)\.

**To create the IAM role for Amazon ECS**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create Role**\.

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. For **Choose the service that will use this role**, choose **Elastic Container Service**\.

1. Under **Select your use case**, choose **Elastic Container Service**, **Next:Permissions**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ecs-role.png)

1. For **Permissions policy**, leave the default policy \(**AmazonEC2ContainerServiceRole**\) selected, and choose **Next:Review**\.

1. For **Role name**, enter a value that helps you identify the role, such as *ECSRoleForEthereum*\. For **Role Description**, enter a brief summary\. Note the role name for later\.

1. Choose **Create role**\.

1. Select the role that you just created from the list\. If your account has many roles, you can search for the role name\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ecs-role-list.png)

1. Copy the **Role ARN** value and save it so that you can copy it again\. You need this ARN when you create the Ethereum network\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ecs-role-arn.png)

The EC2 instance profile that you specify in the template is assumed by EC2 instances in the Ethereum network to interact with other AWS services\. You create a permissions policy for the role, create the role \(which automatically creates an instance profile of the same name\), and then attach the permissions policy to the role\.

**To create an EC2 instance profile**

1. In the navigation pane, choose **Policies**, **Create policy**\.

1. Choose **JSON** and replace the default policy statement with the following JSON policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ecs:CreateCluster",
                   "ecs:DeregisterContainerInstance",
                   "ecs:DiscoverPollEndpoint",
                   "ecs:Poll",
                   "ecs:RegisterContainerInstance",
                   "ecs:StartTelemetrySession",
                   "ecs:Submit*",
                   "ecr:GetAuthorizationToken",
                   "ecr:BatchCheckLayerAvailability",
                   "ecr:GetDownloadUrlForLayer",
                   "ecr:BatchGetImage",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents",
                   "dynamodb:BatchGetItem",
                   "dynamodb:BatchWriteItem",
                   "dynamodb:PutItem",
                   "dynamodb:DeleteItem",
                   "dynamodb:GetItem",
                   "dynamodb:Scan",
                   "dynamodb:Query",
                   "dynamodb:UpdateItem"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. For **Name**, enter a value that helps you identify this permissions policy, for example *EthereumPolicyForEC2*\. For **Description**, enter a brief summary\. Choose **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ec2-perms-policy.png)

1. Choose **Roles**, **Create role**\.

1. Choose **EC2**, **Next: Permissions**\.

1. In the **Search** field, enter the name of the permissions policy that you created earlier, for example *EthereumPolicyForEC2*\.

1. Select the check mark for the policy that you created earlier, and choose **Next: Review**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ec2-select-policy.png)

1. For **Role name**, enter a value that helps you identify the role, for example *EC2RoleForEthereum*\. For **Role description**, enter a brief summary\.Choose **Create role**\.

1. Select the role that you just created from the list\. If your account has many roles, you can enter the role name in the **Search** field\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ec2-select-role.png)

1. Copy the **Instance Profile ARN** value and save it so you can copy it again\. You need this ARN when you create the Ethereum network\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/ec2-role-arn.png)

## Create a Bastion Host<a name="blockchain-templates-bastion-host"></a>

In this tutorial, you create a bastion host\. This is an EC2 instance that you use to connect to the web interfaces and instances in your Ethereum network\. It's sole purpose is to forward SSH traffic from trusted clients outside the VPC so that they can access Ethereum network resources\.

You set up the bastion host because the Application Load Balancer that the template creates is internal, meaning it only routes internal IP addresses\. The bastion host:
+ Has an internal IP address that the Application Load Balancer recognizes because you launch it in the second public subnet that you created earlier\.
+ Has a public IP address that the subnet assigns, which can be accessed by trusted sources outside the VPC\.
+ Is associated with the security group for the Application Load Balancer you created earlier, which has an inbound rule that allows SSH traffic \(port 22\) from trusted clients\.

To be able to access the Ethereum network, trusted clients need to be set up to connect through the bastion host\. For more information, see [Connect to EthStats and EthExplorer Using the Bastion Host](blockchain-bastion-host-connect.md)\. A bastion host is one approach\. You can use any approach that provides access from trusted clients to private resources within a VPC\.

**To create a bastion host**

1. Follow the first five steps to [Launch an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Choose **Edit Instance Details**\. For **Network**, choose the VPC you created earlier, for **Subnet** select the second public subnet that you created earlier\. Leave all other settings to their defaults\.

1. Confirm the change when prompted, and then choose **Review and Launch**\.

1. Choose **Edit Security Groups**\. For **Assign a security group**, choose **Select an existing security group**\.

1. From the list of security groups, select the security group for the Application Load Balancer that you created earlier, and then choose **Review and Launch**\.

1. Choose **Launch**\.

1. Note the instance ID\. You need it later when you [Connect to EthStats and EthExplorer Using the Bastion Host](blockchain-bastion-host-connect.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/bastion-instance.png)