Bitfusion Ubuntu 14 Torch AMI
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


-------------------------------------------------------------------------------

Local Torch CPU & GPU Examples

-------------------------------------------------------------------------------

We have pre-installed Torch examples in the following folder:

  /opt/torch/demos (https://github.com/torch/demos.git)
  /opt/torch/fficudnn (https://github.com/soumith/cudnn.torch.git)

Example 1: CPU

  th /opt/torch/demos/profiling/linear-cpu.lua

Example 2: CPU

  th /opt/torch/demos/profiling/conv-cpu.lua

Example 3: GPU example with cutorch/cunn

  th /opt/torch/demos/profiling/linear-gpu.lua

Example 4: GPU example using cuDNN

  th /opt/torch/fficudnn/test/benchmark.lua


-------------------------------------------------------------------------------

Configuration for Remote GPU Acceleration

-------------------------------------------------------------------------------

While you can utilize this AMI in a standalone configuration on a
CPU based instance, in order to utilize GPUs & Boost you need to connect to a
Bitfusion Boost Server Cluster. The easiest way to deploy this configuration
is  via our Cloudformation template for AWS which can be accessed using this
link:

  http://amzn.to/1RpgWvu

Alternatively you can start the GPU server instance manually using the
the Bitfusion Boost Server GPU AMI:

  https://aws.amazon.com/marketplace/pp/B01BHX6X36

For the manual configuration you need to specify the private IP address of the
server in the Bitfusion Boost configuration file:

 /etc/bitfusionio/adaptor.conf

Please specify one server IP address per line. You can specify multiple Bitfusion
Boost (GPU) servers to potentially obtain increased performance.

Please refer to the following documentation for detailed AWS instructions.

 https://bitfusionio.readme.io/docs/boost-on-aws


-------------------------------------------------------------------------------

Remote Torch GPU Examples with Bitfusion Boost Server

-------------------------------------------------------------------------------

We have pre-installed Torch examples in the following folder:

  /opt/torch/demos (https://github.com/torch/demos.git)
  /opt/torch/fficudnn (https://github.com/soumith/cudnn.torch.git)

Example 1: GPU example with cutorch/cunn

  bfboost client th /opt/torch/demos/profiling/linear-gpu.lua

Example 2: GPU example using cuDNN

  bfboost client th /opt/torch/fficudnn/test/benchmark.lua


-------------------------------------------------------------------------------

Supported AWS Instances

-------------------------------------------------------------------------------

t2.medium	t2.large
m3.medium	m3.large	m3.xlarge	m3.2xlarge
m4.large	m4.xlarge	m4.2xlarge	m4.4xlarge	m4.10xlarge
c3.large	c3.xlarge	c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large	c4.xlarge	c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large	r3.xlarge	r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge	i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge	d2.2xlarge	d2.4xlarge	d2.8xlarge
g2.2xlarge	g2.8xlarge


-------------------------------------------------------------------------------

Version History

-------------------------------------------------------------------------------


v0.05

['Added CFN Bootstrap.  Required for cloudformation based boost clusters.']

v0.04

['Updated bfboost deb package to version 0.1.0+1402 - Boost performance improvements', 'Simpler upstart process for boost servers', 'Jupyter support (Python2 & iTorch)', 'nvcc added to execution path']

v0.03

['Simpler EULA acceptance']

v0.02

['Updated README document']

v0.01

['Nvidia Driver Version 346.46', 'Cuda Toolkit Version 7.0.27', 'Nvidia cuDNN Version 3', 'Torch Version 7', 'bfboost 0.1.0+1190']


-------------------------------------------------------------------------------

Support

-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
