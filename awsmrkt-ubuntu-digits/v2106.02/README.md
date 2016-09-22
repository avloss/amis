Bitfusion Ubuntu 14 Digits v4.0.0 AMI
==============================================================================


Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01DCKFASQ)


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

Jupyter Notebook - http://{ EC2 Instance Public IP }:8888
-------------------------------------------------------------------------------

#### Logging In

You can login to the notebook at:

  * http://{EC2 Instance Public IP}:8888
  * The login PASSWORD is set to the Instance ID.

You can get the Instance ID (Jupyter Notebook Password) from the EC2 console by
clicking on the running instance, or if you are logged in via ssh you can obtain
it by executing the following command:

```
  ec2metadata --instance-id
```


#### Updating the HASHED Jupyter Login Password:

**It is highly recommended that you change the Jupyter login password.**

When logged in via ssh you can update the hashed password using the function
notebook.auth.security.passwd():

```
  ipython
  In [1]: from notebook.auth import passwd
  In [2]: passwd()
  Enter password:
  Verify password:
  Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
  exit()
```

You can then add the hashed password to your Jupyter config file, the default
location for this file is ~/.jupyter/jupyter_notebook_config.py

Example:

```
  c.NotebookApp.password = u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
```

To have the new password take effect restart the Jupyter:

```
  sudo service ipython-notebook restart
```


#### Notebook Location

The default notebook directory is /home/ubuntu/pynb.  This directory is
required for Jupyter to function.  If it is deleted you will need
recreate it and ensure it is owned by the ubuntu user.

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

Using the WebApp:

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

