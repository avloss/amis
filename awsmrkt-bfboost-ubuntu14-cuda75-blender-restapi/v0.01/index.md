```
    ###############################################################################
    ##                                                                           ##
    ## Find this AMI useful and think others may as well? Refer a colleague or a ##
    ##         friend and receive $250 in AWS infrastructure credits.            ##
    ##                                                                           ##
    ##              http://www.bitfusion.io/aws-product-referral/                ##
    ##                                                                           ##
    ###############################################################################

```
Bitfusion Ubuntu 14 Blender Rest API AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](http://www.bitfusion.io/boost-machine-images/)


EC2 Instance Access
-------------------------------------------------------------------------------

To get started, launch an AWS instances using this AMI from the EC2
Console. If you are not familiar with this process please review the AWS
documentation provided here:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html

Accessing the instance via SSH:

```
ssh -i <path to your pem file> ubuntu@{ EC2 Instance Public IP }
```


API Readme & Examples
-------------------------------------------------------------------------------

Please refer to:  https://github.com/bitfusionio/blender_api



Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.medium	t2.large
m3.medium	m3.large	m3.xlarge	m3.2xlarge
m4.large	m4.xlarge	m4.2xlarge	m4.4xlarge	m4.10xlarge
c3.large	c3.xlarge	c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large	c4.xlarge	c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large	r3.xlarge	r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge	i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge	d2.2xlarge	d2.4xlarge	d2.8xlarge
g2.2xlarge	g2.8xlarge
```

Version History
-------------------------------------------------------------------------------


v0.01

 * Blender 2.77
 * Nvidia Driver Version 352
 * Cuda Toolkit Version 7.5
 * Nvidia cuDNN Version 4
 * bfboost 0.1.0+1402




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
