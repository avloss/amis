Bitfusion Ubuntu 14 Digits v4.0.0 AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01DJ93C7Q)


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

## Nvidia Digits

The NVIDIA Deep Learning GPU Training System (DIGITS) puts the power of deep
learning in the hands of data scientists and researchers. Quickly design the
best deep neural network (DNN) for your data using real-time network behavior
visualization.


#### Security

Please take a moment to review your security groups to prevent public
access to the Digits WebApp and SSH port.  As a recommendation you should
only allow access to the Digits WebApp and SSH port from your IP

Please review the AWS documents on authorizing access to your systems:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#adding-security-group-rule


#### Getting Started with Digits


This system comes installed with Digits and the following machine learning
packages:

 - caffe-nv
 - pythong-caffe-nv
 - torch-nv
 - libcudnn5
 - Cuda Toolkit 7.5
 - Nvidia Driver 352.99

##### Downloading Datasets

In order to go through the getting started guide you need to download the datasets by logging into the system
and using the bundled python script.

To download the mnist dataset:

```
 cd /home/ubuntu
 /usr/share/digits/tools/download_data/main.py mnist mnist
```

To download the cifar10 dataset:

```
 cd /home/ubuntu
 /usr/share/digits/tools/download_data/main.py cifar10 cifar10
```

To download the cifar100 dataset:

```
 cd /home/ubuntu
 /usr/share/digits/tools/download_data/main.py cifar100 cifar100
```

##### Using the WebApp:

    The digits webserver will be running on the public IP of the EC2 instance.
    Open up a web browser and navigate to the Digits home screen:

    http://<EC2_INSTANCE_PUBLIC_IP>

Nvidia getting started guide:

https://github.com/NVIDIA/DIGITS/blob/master/docs/GettingStarted.md#using-the-webapp


## Supported & Tested AWS Instances

g2.2xlarge	g2.8xlarge


Version History
-------------------------------------------------------------------------------


v2016.02

 * Updated to Digits 4.0
 * Updated to NVIDIA cuDNN Version 5.1
 * Updated to NVIDIA Driver 352.99


v0.01

 * Digits 3.0
 * Cuda Toolkit Version 7.5
 * NVIDIA cuDNN Version 4
 * NVIDIA Driver 352.79




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io

