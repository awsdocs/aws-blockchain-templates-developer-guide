# Clean Up Resources<a name="blockchain-templates-cleanup"></a>

AWS CloudFormation makes it easy to clean up resources that the stack created\. When you delete the stack, all resources that the stack created are deleted\.

**To delete resources that the template created**
+ Open the AWS CloudFormation console, select the root stack that you created earlier, choose **Actions**, **Delete**\.

  The **Status** of the root stack you created earlier and the associated nested stacks update to **DELETE\_IN\_PROGRESS**\.

You may choose to delete the prerequisites you created for the Ethereum network\.

**Delete the VPC**
+ Open the Amazon VPC console, select the VPC you created earlier and then choose **Actions**, **Delete VPC**\. This also deletes the subnets, security groups, and the NAT gateway associated with the VPC\.

**Delete the IAM role and EC2 instance profile**
+ Open the IAM console and choose **Roles**\. Select the role for ECS and the role for EC2 that you created earlier and choose **Delete**\.

**Terminate the EC2 instance for the bastion host**
+ Open the Amazon EC2 dashboard, choose **Running instances**, select the EC2 instance that you created for the bastion host, choose **Actions**, **Instance State**, **Terminate**\.