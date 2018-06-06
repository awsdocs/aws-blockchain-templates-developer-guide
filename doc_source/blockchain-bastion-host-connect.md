# Connect to EthStats and EthExplorer Using the Bastion Host<a name="blockchain-bastion-host-connect"></a>

To connect to Ethereum resources in this tutorial, you set up SSH port forwarding \(SSH tunneling\) through the bastion host\. The following instructions demonstrate how to do this so that you can connect to EthStats and EthExplorer URLs using a browser\. In the instructions below, you first set up a SOCKS proxy on a local port\. You then use a browser extension, [FoxyProxy](https://getfoxyproxy.org/), to use this forwarded port for your Ethereum network URLs\.

If you use Mac OS or Linux, use an SSH client to set up the SOCKS proxy connection to the bastion host\. If you are a Windows user, use PuTTY\. Before you connect, confirm that the client computer you are using is specified as an allowed source for inbound SSH traffic in the security group for the Application Load Balancer that you set up earlier\.

**To connect to the bastion host with SSH port forwarding using SSH**
+ Follow the procedures in [Connecting to Your Linux Instance Using SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) in the *Amazon EC2 User Guide for Linux Instances*\. For step 4 of the [Connecting to Your Linux Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html#AccessingInstancesLinuxSSHClient) procedure, add `-D 9001` to the SSH command, specify the same key pair that you specified in the AWS Blockchain Template for Ethereum configuration, and specify the DNS name of the bastion host\.

  ```
  ssh -i /path/my-template-key-pair.pem ec2-user@bastion-host-dns -D 9001
  ```

**To connect to the bastion host with SSH port forwarding using PuTTY \(Windows\)**

1. Follow the procedures in [Connecting to Your Linux Instance from Windows Using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon EC2 User Guide for Linux Instances* through step 7 of the [Starting a PuTTY Session](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html#putty-ssh) procedure, using the same key pair that you specified in the AWS Blockchain Template for Ethereum configuration\.

1. In PuTTY, under **Category**, choose **Connection**, **SSH**, **Tunnels**\.

1. For **Port forwarding**, choose **Local ports accept connections from other hosts**\.

1. Under **Add new forwarded port**:

   1. For **Source port**, enter **9001**\. This is an arbitrary unused port that we chose, and you can choose a different one if necessary\.

   1. Leave **Destination** blank\.

   1. Select **Dynamic**\.

   1. Choose **Add**\.

   For **Forwarded ports**, **D9001** should appear as shown below\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/blockchain-templates/latest/developerguide/images/putty.png)

1. Choose **Open** and then authenticate to the bastion host as required by your key configuration\. Leave the connection open\.

With the PuTTY connection open, you now configure your system or a browser extension to use the forwarded port for your Ethereum network URLs\. The following instructions are based on using FoxyProxy Standard to forward connections based on the URL pattern of EthStats and EthExplorer and port 9001, which you established earlier as the forwarded port, but you can use any method that you prefer\.

**To configure FoxyProxy to use the SSH tunnel for Ethereum network URLs**

1. Download and install the FoxyProxy Standard browser extension, and then open **Options** according to the instructions for your browser\.

1. Choose **Add New Proxy**\.

1. On the **General** tab, make sure that **Enabled** is selected and enter a **Proxy Name** and **Proxy Notes** that help you identify this proxy configuration\.

1. On the **Proxy Details** tab, choose **Manual Proxy Configuration**\. For **Host or IP Address** \(or **Server or IP Address** in some versions\), enter *localhost*\. For **Port**, enter *9001*\. Select **SOCKS Proxy?**.

1. On the **URL Pattern** tab, choose **Add New Pattern**\.

1. For **Pattern name**, enter a name that's easy to identify, and for **URL Pattern**, enter **http://internal\-stackname\-loadb\***\.

1. Leave the default selections for other settings and choose **Save**\.

You are now able to connect to the Ethereum URLs, which are available on CloudFormation console using the **Outputs** tab of the root stack that you created with the template\.
