# Setting Up AWS Blockchain Templates<a name="blockchain-templates-setting-up"></a>

Before you start with AWS Blockchain Templates, complete the following tasks:
+ [Sign Up for AWS](#blockchain-templates-sign-up-for-aws)
+ [Create an IAM User](#blockchain-templates-create-iam-user)
+ [Create a Key Pair](#blockchain-templates-create-a-key-pair)

These are fundamental prerequisites for all blockchain configurations\. In addition, the blockchain network that you choose may have prerequisites, which vary according to your desired environment and configuration choices\. For more information, see the relevant section for your blockchain template in [AWS Blockchain Templates and Features](blockchain-template-features.md)\.

For step\-by\-step instructions to set up prerequisites for a private Ethereum network using an Amazon ECS cluster, see [Getting Started with AWS Blockchain Templates](blockchain-templates-getting-started.md)\.

## Sign Up for AWS<a name="blockchain-templates-sign-up-for-aws"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services\. You are charged only for the services that you use\.

If you have an AWS account already, skip to the next task\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign in to a different account**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

Note your AWS account number\. You need it when you create an IAM user in the next task\.

## Create an IAM User<a name="blockchain-templates-create-iam-user"></a>

Services in AWS require that you provide credentials when you access them, so that the service can determine whether you have permissions to access its resources\. The console requires your password\. You can create access keys for your AWS account to access the command line interface or API\. However, we don't recommend that you access AWS using the credentials for your AWS account; we recommend that you use AWS Identity and Access Management \(IAM\) instead\. Create an IAM user, and then add the user to an IAM group with administrative permissions or grant this user administrative permissions\. You can then access AWS using a special URL and the credentials for the IAM user\.

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\. If you already have an IAM user, you can skip this step\.

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](http://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type **Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions for user** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, type **Administrators**\.

1. For **Filter**, choose **Job function**\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

To sign in as this new IAM user, sign out of the AWS Management Console, then use the following URL, where *your\_aws\_account\_id* is your AWS account number without the hyphens \(for example, if your AWS account number is `1234-5678-9012`, your AWS account ID is `123456789012`\):

```
https://your_aws_account_id.signin.aws.amazon.com/console/
```

Enter the IAM user name and password that you just created\. When you're signed in, the navigation bar displays "*your\_user\_name* @ *your\_aws\_account\_id*"\.

If you don't want the URL for your sign\-in page to contain your AWS account ID, you can create an account alias\. From the IAM dashboard, choose **Create Account Alias** and enter an alias, such as your company name\. To sign in after you create an account alias, use the following URL:

```
https://your_account_alias.signin.aws.amazon.com/console/
```

To verify the sign\-in link for IAM users for your account, open the IAM console and check under **IAM users sign\-in link** on the dashboard\.

For more information, see the [AWS Identity and Access Management User Guide](http://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

## Create a Key Pair<a name="blockchain-templates-create-a-key-pair"></a>

AWS uses public\-key cryptography to secure the login information for the instances in a blockchain network\. You specify the name of the key pair when you use each AWS Blockchain Template\. You can then use the key pair to access instances directly, for example, to log in using SSH\. 

If you already have a key pair in the right Region, you can skip this step\. If you haven't created a key pair already, you can create one using the Amazon EC2 console\. Create the key pair in the same Region that you use to launch the Ethereum network\. For more information, see [Regions and Availability Zones](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

**To create a key pair**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select a Region for the key pair\. You can select any Region that's available to you, regardless of your location, but key pairs are specific to a Region\. For example, if you plan to launch an instance in the US East \(Ohio\) region, you must create a key pair for the instance in the same Region\.

1. In the navigation pane, choose **Key Pairs**, **Create Key Pair**\.

1. For **Key pair name**, enter a name for the new key pair\. Choose a name that is easy for you to remember, such as your IAM user name, followed by `-key-pair`, plus the region name\. For example, *me*\-key\-pair\-*useast2*\. Choose **Create**\.

1. The private key file is automatically downloaded by your browser\. The base file name is the name that you specified as the name of your key pair, and the file name extension is `.pem`\. Save the private key file in a safe place\.
**Important**  
This is the only chance for you to save the private key file\. You provide the name of your key pair when you launch the Ethereum network\.

For more information, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\. For more information about connecting to EC2 instances using the key pair, see [Connect to Your Linux Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.