Bitfusion Ubuntu 14 MXNet AMI
==============================================================================



#### Contact Us

* [Join us on Slack](https://slack-bitfusion-aws.herokuapp.com/)  [![](https://slack.global.ssl.fastly.net/272a/img/icons/favicon-16.png)](https://slack-bitfusion-aws.herokuapp.com/)

* [Send us a note](http://www.bitfusion.io/support/)                                                                                                                                      





Getting started - Launch the AMI
-------------------------------------------------------------------------------

Subscribe to and launch the AMI from here:

[Launch the AMI](https://aws.amazon.com/marketplace/pp/B01EYKBEQ0)


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

MXNet - http://mxnet.io/
------------------------

For more information and great tutorials please visit http://mxnet.io/tutorials/index.html


### MXNet Core Concept

Flexible
* Supports both imperative and symbolic programming

Portable
* Runs on CPUs or GPUs, on clusters, servers, desktops, or mobile phones

Multiple Languages
* Supports over 7 programming languages, including C++, Python, R, Scala, Julia, Matlab, and Javascript

Auto-Differentiation
* Calculates the gradient automatically for training a model

Distributed on Cloud
* Supports distributed training on multiple CPU/GPU machines, including AWS, GCE, Azure, and Yarn clusters

Performance
* Optimized C++ backend engine parallelizes both I/O and computation

Sited from the website:

```
MXNet is an open-source deep learning framework that allows you to define, train,
and deploy deep neural networks on a wide array of devices, from cloud infrastructure
to mobile devices. It is highly scalable, allowing for fast model training, and
supports a flexible programming model and multiple languages. MXNet allows you to mix
symbolic and imperative programming flavors to maximize both efficiency and productivity.
MXNet is built on a dynamic dependency scheduler that automatically parallelizes both
symbolic and imperative operations on the fly. A graph optimization layer on top of that
makes symbolic execution fast and memory efficient. The MXNet library is portable and
lightweight, and it scales to multiple GPUs and multiple machines.

```

### MXNet Example Notebooks

The MXnet example notebooks are bundled with the AMI and are available via Jupyter

You can login and work through the notebooks by navigating to the url below.  For more information 
on using Jupyter with this AMI, please refer to the Jupyter section

  * `http://{EC2 Instance Public IP}:8888/mxnet/examples/notebooks`
  
##### Notebook Issues
> In order to run use the cifar-100 notebook, you will need to run the cifar10-recipe notebook to
> obtain the image data

> The cifar10-recipe has a bug in the notebook. There is a bad path reference to the cifar data.
> To fix it, update cell 9 and change any references to "data/cifar/" to "data/"


### Verify MXNet is installed
```
python
>>> import mxnet
>>> mxnet.__version__
'0.7.0'
```
 
 
### MXNet GPU Training Example

``` 
cd /home/ubuntu/mxnet

# Python 2
python example/image-classification/train_mnist.py --network lenet --gpus 0

# Python 3
python3 example/image-classification/train_mnist.py --network lenet --gpus 0
```


### MXNet CPU Training Example

Simply remove the GPU argument at the end
```
cd /home/ubuntu/mxnet
python example/image-classification/train_mnist.py --network lenet

```



Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.nano     t2.micro    t2.medium   t2.large
m3.medium   m3.large    m3.xlarge   m3.2xlarge
m4.large    m4.xlarge   m4.2xlarge  m4.4xlarge  m4.10xlarge  m4.16xlarge
c3.large    c3.xlarge   c3.2xlarge  c3.4xlarge  c3.8xlarge
c4.large    c4.xlarge   c4.2xlarge  c4.4xlarge  c4.8xlarge
r3.large    r3.xlarge   r3.2xlarge  r3.4xlarge  r3.8xlarge
i2.xlarge   i2.2xlarge  i2.4xlarge  i2.8xlarge
d2.xlarge   d2.2xlarge  d2.4xlarge  d2.8xlarge
g2.2xlarge  g2.8xlarge
p2.xlarge   p2.8xlarge  p2.16xlarge
x1.16xlarge x1.32xlarge
```

Version History
-------------------------------------------------------------------------------


v2016.01

 * Enum34 1.1.6 (Python 2 and Python 3)
 * H5py 2.6.0 (Python2 and Python3 modules)
 * MXNet 0.7.0 (Python2 and Python3 modules)
 * Matplotlib to 1.5.3 (Python2 and Python2 modules)
 * nltk 3.2.1 (Python2 and Python3 modules)
 * Numpy 1.11.1 (Python2 and Python3 modules)
 * SciPy 0.18.0 (Python2 and Python3 modules)
 * Pandas 0.19.1 (Python2 and Python3 modules)
 * SpaCy 1.0 (Python2 and Python3 modules)
 * Sympy 1.0 (Python2 and Python3 modules)
 * Scikit-Learn 0.18.1 (Python2 and Python3 modules)
 * PyCuda 2016.1.2 (Python 2 and Python 3)
 * Jupyter, NBExtensions and support to compile pycuda kernels
 * Cuda 7.5
 * cuDNN 5.1
 * Nvidia Driver 352.99
 * GPU Stat




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io

[Join us on Slack](https://slack-bitfusion-aws.herokuapp.com/)  [![](https://slack.global.ssl.fastly.net/272a/img/icons/favicon-16.png)](https://slack-bitfusion-aws.herokuapp.com/)
[Contact Us](http://www.bitfusion.io/support/)           
