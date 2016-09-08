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

## Bitfusion Blender AMI with Rest API

This Ubuntu 14 AMI comes pre-installed with Nvidia Drivers, Cuda 7.5 Toolkit, and Blender 2.77. It is optimized for Nvidia GRID GPU instances as well as CPU instances. The AMI can be used Integrate compute intensive rendering directly into your applications and workflows from mobile devices or simple clients through an easy to use RestAPI.

Achieve orders of magnitude faster rendering performance while saving battery power. Example benchmarks achieved on popular Blender benchmarks:

### Classroom Benchmarks

|use-case                     | render-time  |  tile-size  |     render device  |
|-----------------------------|--------------|-------------|--------------------|
|Tablet (Local: i5-4200U):    |   12660s     |  (16x16)    |     laptop         |
|Tablet (Remote via RestAPI): |   3763s      |  (16x16)    |     g2.8xlarge     |
|Tablet (Remote via RestAPI): |   1329s      |  (32x32)    |     g2.8xlarge     |
|Tablet (Remote via RestAPI): |   464s       |  (64x64)    |     g2.8xlarge     |
|Tablet (Remote via RestAPI): |   342s       |  (128x128)  |     g2.8xlarge     |
|Tablet (Remote via RestAPI): |   335s       |  (256x256)  |     g2.8xlarge     |

### Other Examples:
BMW Benchmark:
* 28x faster (cpu tiles-size: 16x16, gpu tile-size: 128x128)
* Tablet (local render on i5-4200 cpu): 1200s
* Tablet (remote REST API render on g2.8xlarge instance): 43s

Splash-Pokedstudio Benchmark:
* 17x faster (cpu tiles-size: 32x32, gpu tile-size: 128x128)
* Tablet (local render on i5-4200 cpu): 5314s
* Tablet (remote REST API render on g2.8xlarge instance): 312s


## Bitfusion Blender API


| METHOD       | URI                                 | ACTION                                            |
|--------------|-------------------------------------|---------------------------------------------------|
| POST         | http://[hostname]/input/            | Upload a zip file containing blendfile and assets |
| GET          | http://[hostname]/input/            | List all uploaded zip files                       |
| DELETE       | http://[hostname]/input/{file_name} | Delete an uploaded zip file                       |
| POST         | http://[hostname]/job/              | Submit a blender job & return a task ID           |
| GET          | http://[hostname]/job/{id}          | Get job status based on the returned task ID      |
| GET          | http://[hostname]/output/           | List output files available                       |
| GET          | http://[hostname]/output/file_name  | Download the output file                          |
| DELETE       | http://[hostname]/output/file_name  | delete output file                                |

#### Walkthrough of how to submit a job:

Below will walk you through the process of uploading a zip file, submitting a job, then retrieving it.  Before running
any of the scripts modify the variables in the config section of the script accordingly.

We have also included an example script that will go through the entire process described below.

 * https://github.com/bitfusionio/blender_api/blob/master/example.sh

##### Step 1 - Upload Zip file

```
#!/usr/bin/env bash

token="<replace with ec2 instance_id>"
host_ip="<replace with ec2 public ip address>"
file="BMW27.blend.zip"  # Download from: https://download.blender.org/demo/test/BMW27.blend.zip

curl -X POST \
-H "X-Auth-Token: ${token}" \
-H "Cache-Control: no-cache" \
-H "Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW" \
-F "file=@${file}" \
http://${host_ip}:5000/input/

# Expected output:
# {
#     "filename": "BMW27.blend.zip",
#     "status": "success"
# }

```


##### Step 2 - Submit a job.
  * When you submit a job we append our arguments file to force GPU or CPU processing based on the system.  What this means is that we if you are running on a g2.2xlarge or a g2.8xlarge we will enable GPU processing.
    * Job with the following parameters:
    * input_file (e.g. BMW27.blend)
    * zip_file (e.g. BMW27.blend.zip)
    * blender_args (e.g -f 1).
  * We return the generated task ID for future lookup
```
#!/usr/bin/env bash

token="<replace with ec2 instance_id>"
host_ip="<replace with ec2 public ip address>"
file="BMW27.blend"
zipfile="BMW27.blend.zip"

curl -X POST \
  -H "X-Auth-Token: ${token}" \
  -H "Cache-Control: no-cache" \
  -H "Content-Type: application/json" \
  -d '{"zip_file": "'${zipfile}'", "input_file": "'${file}'", "blender_args": "-f 1" }' \
  http://${host_ip}:5000/job/


# Expected output:
# {
#   "input_file": "BMW27.blend",
#   "messege": "Job submitted",
#   "status": "suceeded",
#   "task_id": "890e7930-ab3c-4cfd-a51b-2c1c52b1c11d",
#   "zip_file": "BMW27.blend.zip"
# }
```



##### Step 3 - Use the task ID to get the status of the job.
  * Returns the following:
    * state: failed, pending, success
    * If the job succeeded it returns information about the job:
      * cmd: command used
      * cmd_err: Any output directed to stderr
      * cmd_output:  This is the stand blender job output
      * cmd_return_code:  The return code of the command.  0 means it completed successfully
      * output_file: This will be the output file with the task id appended
      * zip_info:  This will have information about the zip file upaloded
      * state:  This is state of the worker job.

```
#!/usr/bin/env bash

token="<replace with ec2 instance_id>"
host_ip="<replace with ec2 public ip address>"
job_id="8c994d30-5e65-495f-8589-a63f541b1161"

curl -X GET \
  -H "Content-Type: application/json" \
  -H "X-Auth-Token: ${token}" \
  -H "Cache-Control: no-cache" \
  "http://${host_ip}:5000/job/${job_id}"


# Expected output when the job is running:

# {
#     "status": "pending"
# }

# Expected output when the job completes:

# {
#     "result": {
#         "cmd": "blender -b /opt/bf_api/ami_api_blender/uploads/BMW27.blend -o //../output/BMW27-3e2e46a2-5e9b-4686-b460-3630c6bf3a40-##.png -P resources/gpu_render.py -f 1",
#         "cmd_err": "ALSA lib confmis.....,
#         "cmd_output": "Read new prefs: /root/.config.....",
#     "cmd_return_code": 0,
#     "output_file": "BMW27-890e7930-ab3c-4cfd-a51b-2c1c52b1c11d-01.png",
#     "task_id": "890e7930-ab3c-4cfd-a51b-2c1c52b1c11d",
#     "zip_info": {
#       "comment": "",
#       "compressed:": "118 bytes",
#       "filename": "BMW27.blend.zip",
#       "full_path": "/opt/bf_api/ami_api_blender/uploads/BMW27.blend.zip",
#       "system": "3 (0 = Windows, 3 = Unix)",
#       "uncompressed": "201 bytes",
#       "zip_version": 21
#     }
#   },
#   "state": "SUCCESS"
# }
```

##### Step 4 - Request the output_file returned to you in step 3

```
#!/usr/bin/env bash

token="<replace with ec2 instance_id>"
host_ip="<replace with ec2 public ip address>"
output_file="BMW27-890e7930-ab3c-4cfd-a51b-2c1c52b1c11d-01.png"

curl -O -X GET \
 -H "Content-Type: application/json" \
 -H "X-Auth-Token: ${token}" \
 -H "Cache-Control: no-cache" \
 "http://52.87.180.179:5000/output/${output_file}"

# Expected output
# The rendered file will be saved locally
```


Supported AWS Instances
-------------------------------------------------------------------------------
```
t2.micro    t2.medium   t2.large
m3.medium   m3.large    m3.xlarge    m3.2xlarge
m4.large    m4.xlarge   m4.2xlarge   m4.4xlarge  m4.10xlarge
c3.large    c3.xlarge   c3.2xlarge   c3.4xlarge  c3.8xlarge
c4.large    c4.xlarge   c4.2xlarge   c4.4xlarge  c4.8xlarge
r3.large    r3.xlarge   r3.2xlarge   r3.4xlarge  r3.8xlarge
i2.xlarge   i2.2xlarge  i2.4xlarge   i2.8xlarge
d2.xlarge   d2.2xlarge  d2.4xlarge   d2.8xlarge
g2.2xlarge  g2.8xlarge
```

Version History
-------------------------------------------------------------------------------


v2016.02

 * Updated to Blender 2.78
 * Updated to Nvidia Driver Version 352.99
 * Updated to Nvidia cuDNN Version 5.1
 * Updated to bfboost 0.1.0+1562
 * Minor documentation fixes


v0.01

 * Blender 2.77
 * Nvidia Driver Version 352
 * Cuda Toolkit Version 7.5
 * Nvidia cuDNN Version 4
 * bfboost 0.1.0+1402




Support
-------------------------------------------------------------------------------

Please send all comments and support request to support@bitfusion.io
