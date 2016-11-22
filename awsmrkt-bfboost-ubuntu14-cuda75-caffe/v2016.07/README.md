Bitfusion Boost Ubuntu 14 Caffe AMI
==============================================================================



#### Contact Us

* [Join us on Slack](https://slack-bitfusion-aws.herokuapp.com/)  [![](https://slack.global.ssl.fastly.net/272a/img/icons/favicon-16.png)](https://slack-bitfusion-aws.herokuapp.com/)

* [Send us a note](http://www.bitfusion.io/support/)                                                                                                                                      





Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01B52CMSO)


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

  * `http://{EC2 Instance Public IP}:8888`
  * The login PASSWORD is set to the Instance ID.

You can get the Instance ID (Jupyter Notebook Password) from the EC2 console by
clicking on the running instance, or if you are logged in via ssh you can obtain
it by executing the following command:

```
  ec2metadata --instance-id
```


#### Updating the HASHED Jupyter Login Password:

**It is highly recommended that you change the Jupyter login password.**

When logged in via ssh you can update the hashed password using one of the following functions:

 * for iPython 5 ```IPython.lib.passwd```
 * for any earlier release of iPython ```notebook.auth.security.passwd()```:

iPython 5 example:
```
  ipython
  In [1]: from IPython.lib import passwd
  In [2]: passwd()
  Enter password:
  Verify password:
  Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
  exit()
```

iPython 4 and earlier example:
```
  ipython
  In [1]: from notebook.auth import passwd
  In [2]: passwd()
  Enter password:
  Verify password:
  Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
  exit()
```

You can then add the outputed hashed password, which should look similar to ```sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed```
,to your Jupyter config file. The default location for this file is ~/.jupyter/jupyter_notebook_config.py. If you scroll
to the bottom of the file you will see the configuration entry that needs to be updated:

Example:

```
  c.NotebookApp.password = u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
```

Place your update string in place the old one and restart Jupyter to have the password take effect.

```
  sudo service jupyter restart
```


#### Notebook Location

The default notebook directory is /home/ubuntu/pynb.  This directory is
required for Jupyter to function.  If it is deleted you will need
recreate it and ensure it is owned by the ubuntu user.

PyCaffe
-------------------------------------------------------------------------------

We have compiled and installed PyCaffe for both CPUs and GPUs on this AMI. When 
the system boots we determine the instance type and set the path appropriatley.

#### PyCaffe Import Warning:

    During an import you may see the following warning:
```
   /home/ubuntu/caffe/python/caffe/pycaffe.py:13: RuntimeWarning: to-Python converter for boost::shared_ptr<caffe::Net<float> > already registered; second conversion method ignored.
     from ._caffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, \
   /home/ubuntu/caffe/python/caffe/pycaffe.py:13: RuntimeWarning: to-Python converter for boost::shared_ptr<caffe::Blob<float> > already registered; second conversion method ignored.
     from ._caffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, \
   /home/ubuntu/caffe/python/caffe/pycaffe.py:13: RuntimeWarning: to-Python converter for boost::shared_ptr<caffe::Solver<float> > already registered; second conversion method ignored.
     from ._caffe import Net, SGDSolver, NesterovSolver, AdaGradSolver, \
```

This is a harmless warning due to differences in boost versions, more info
here:

 * https://groups.google.com/forum/#!topic/caffe-users/C_air48cISU/discussion


Local Caffe Example
-------------------------------------------------------------------------------

We have pre-installed Caffe and and examples in ```/home/ubuntu/caffe```.


##### Example - Training LeNet on MNIST with Caffe:

Readme: ```/home/ubuntu/caffe/examples/mnist/readme.md```

You will first need to download and convert the data format from the MNIST
website. To do this, run the following commands:

```
  cd /home/ubuntu/caffe
  ./data/mnist/get_mnist.sh
  ./examples/mnist/create_mnist.sh
```

Ensure the solver is set to CPU mode by editing the following file:

```
  vi examples/mnist/lenet_solver.prototxt
```

And make sure the solver line is as follows:

```
   solver_mode: CPU
```
To run the test:

```
    ./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt
```

Local Caffe GPU Example
-------------------------------------------------------------------------------

We have pre-installed Caffe and and examples in ```/home/ubuntu/caffe```.


#### Example - Training LeNet on MNIST with Caffe:

Readme:   ```/home/ubuntu/caffe/examples/mnist/readme.md```

You will first need to download and convert the data format from the MNIST
website. To do this, run the following commands:

```
  cd /home/ubuntu/caffe
  ./data/mnist/get_mnist.sh
  ./examples/mnist/create_mnist.sh
```

Ensure the solver is set to GPU mode by editing the following file:

```
  vi examples/mnist/lenet_solver.prototxt
```

And make sure the solver line is as follows:

```
  solver_mode: GPU
```

To run the test:

```
  ./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt -gpu all
```

Bitfusion Boost Caffe GPU Cluster
-------------------------------------------------------------------------------

To start this AMI in a Boost Cluster formation select one of the cluster
configuration for this AMI from the AWS Marketplace:

https://aws.amazon.com/marketplace/pp/B01B52CMSO

Once the Boost Cluster is up and running you can utilize Boost to execute your
application across all the nodes in the cluster as follows:

```
  bfboost client <command>
```

For example you can execute device query as follows:

```
  bfboost client  /usr/local/cuda/samples/bin/x86_64/linux/release/deviceQuery
```

Example - Training LeNet on MNIST with Caffe:

Readme: /home/ubuntu/caffe/examples/mnist/readme.md

You will first need to download and convert the data format from the MNIST
website. To do this, run the following commands:

```
  cd /home/ubuntu/caffe

  ./data/mnist/get_mnist.sh
  ./examples/mnist/create_mnist.sh
```

To run the test:

```
  bfboost client "./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt -gpu all"
```

Detailed Boost and Boost Cluster documentaion is available at:

  http://www.bitfusion.io/documentation-boost-aws/


Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.nano     t2.micro    t2.medium   t2.large
m4.large    m4.xlarge	m4.2xlarge	m4.4xlarge	m4.10xlarge  m4.16xlarge
c3.large    c3.xlarge	c3.2xlarge	c3.4xlarge	c3.8xlarge
c4.large    c4.xlarge	c4.2xlarge	c4.4xlarge	c4.8xlarge
r3.large    r3.xlarge	r3.2xlarge	r3.4xlarge	r3.8xlarge
i2.xlarge   i2.2xlarge	i2.4xlarge	i2.8xlarge
d2.xlarge   d2.2xlarge	d2.4xlarge	d2.8xlarge
g2.2xlarge  g2.8xlarge
p2.2xlarge  p2.8xlarge  p2.16xlarge
x1.16xlarge x1.32xlarge
```

Version History
-------------------------------------------------------------------------------


v2016.07

 * Migrated to user install of caffe v1.0.0-rc3
 * Systems now determines if the instance type has GPUs. If so it is set to the path to caffe compiled with CUDA
 * Jupyter should work on both CPU and GPU instance types
 * Added Caffe example notebooks to Jupyter
 * Added Bitfusion README Python Notebook
 * Enum34 (Python2 and Python3 modules)
 * h5py 2.6.0 (Python2 and Python3 modules)
 * Matplotlib 1.5.3 (Python2 and Python3 modules)
 * NumPy 1.11.1 (Python2 and Python3 modules)
 * Pandas 0.19.1 (Python2 and Python3 modules)
 * PyDot 1.1.0 (Python2 and Python3 modules)
 * SciPy 0.18.0 (Python2 and Python3 modules)
 * SymPy 1.0 (Python2 and Python3 modules)
 * Added PyCuda 2016.1.2 (Python 2 and Python 3)
 * Added Python Notebook Extensions


v2016.06

 * Updated to latest version Bitfusion Boost 0.1.0+1562
 * Updated to latest nvidia driver 352.99
 * Updated CuDNN to 5.1
 * Updated to the latest version of caffe (SHA 985493e9ce3e8b61e06c072a16478e6a74e3aa5a)
 * Added gpustat


v0.05

 * Upgrade to cudnn 5
 * Upgrade to nvidia driver 352
 * Upgrade to cuda 7.5 toolkit
 * Upgrade to the latest version of boost 0.1.0+1518
 * Upgrade to the latest version of caffe


v0.04

 * Added CFN Bootstrap.  Required for cloudformation based boost clusters.


v0.03

 * Updated bfboost to version 0.1.0+1402, performance improvements
 * simpler upstart process for servers
 * Jupyter support
 * Python OpenCV support
 * nvcc added to execution path
 * {'Bugfix': 'odd number GPUs - merged https://github.com/BVLC/caffe/pull/3586'}
 * {'Bugfix': 'pycaffe gpu support'}


v0.02

 * Added pycaffe support


v0.01

 * Nvidia Driver Version 346.46
 * Cuda Toolkit Version 7.0.27
 * Nvidia cuDNN Version 3
 * bfboost 0.1.0+1190




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io

[Join us on Slack](https://slack-bitfusion-aws.herokuapp.com/)  [![](https://slack.global.ssl.fastly.net/272a/img/icons/favicon-16.png)](https://slack-bitfusion-aws.herokuapp.com/)
[Contact Us](http://www.bitfusion.io/support/)           
