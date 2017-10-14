# Google Cloud for Deep Learning
This small tutorial is a little help for students, who are participating in "DEEP LEARNING A GYAKORLATBAN PYTHON ÉS LUA ALAPON" at Budapest University of Technology and Economics.

If you have any question, please feel free to contact me! All suggestions are welcome!
## GCP Free Tier
Table of content:
1. How to register
2. Setup Google Cloud SDK
3. Google Cloud Compute - Virtual Machines
    1. How to start an instance
    2. Useful commands 
4. Datalab (WIP)

**WARNING: The registration, and the usage of the services is free for a limited amount of duration, and resource, BUT only for this duration.** 
### How to register
Register with your Google account [here.](https://cloud.google.com/free/)
- Registration is easy, but **credit card is needed** (or paypal).
- For 'company' just use 'Personal Usage Corp.','HomeResearch' or something like that.
- After the registration e-mail we will have an access to GCloud.

## Setup Google Cloud SDK
It's much easier to use GCloud with a CLI (command line interface) than with a browser. I highly recommend to use the SDK.

On linux (Ubuntu 16.04LTS):
[Quick Start Guide by Google](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)

Installing of the SDK:
```
# Create an environment variable for the correct distribution
export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"

# Add the Cloud SDK distribution URI as a package source
echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

# Import the Google Cloud Platform public key
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

# Update the package list and install the Cloud SDK
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

Initialization of the SDK on our system:

Type `gcloud init` and login to your google account (browser needed).
After successful login choose between projects (or creat a new one with the CLI).

## Google Cloud Compute - Virtual Machines
So google cloud instance is just a virtual machine (just like in AWS), but you have much more freedom to scale it. For Deep Learning it's a necessary to use GPU (it's event more important with bigger models, and with more data), therefore we will see how can we setup an instance with GPU. **WARNING: I want to repeat myself, that this is not free (however you have free credits).**

### How to start an instance
Because GUI is much more interactive, I will show this process with web-GUI on Google Cloud Management Console:
* Go to [Google Cloud Management Console](https://console.cloud.google.com/home)
* Open the hamburger menu (from upper left corner), and find **Compute / Compute Engine**
* On the top menu you can find an option: **CREATE INSTANCE** - click on it!

Now we need to define the properties of our instance:
* Wee need GPU, so change our location, where it's available:
    * NVIDIA® Tesla® P100: us-west1-b, europe-west1-b, ...
    * NVIDIA® Tesla® K80: us-west1-b, europe-west1-b, ... (full list available [here](https://cloud.google.com/compute/docs/gpus/) )
    * **GPU pricing [here](https://cloud.google.com/compute/pricing#gpus).**
* Under **Machine Type** click to "Costumize" (this will open a detailed view for this)
* On the detailed view, click on "GPUs" (on the bottom), then you can set the desired number of GPUs, and GPU type.

 [Instance Creation Image](https://github.com/szdani/gcloud-dl/blob/master/image.png)

* For Boot disk I recommend to use Ubuntu LTS (16.04), especially if you are not familiar with linux
* If you want to use it via SSH, don't forget to add a key under SSH Keys option

**BEFORE YOU WANT TO FINISH WITH THE "Create"  BUTTON, PLEASE TAKE A LOOK AT THE ESTIMATED PRICE ON THE PAGE**

If the instance is created, and running, you can connect to it via SSH, or with CLI. To connect with CLI just type the following command:
`gcloud compute ssh [INSTANCE NAME]`

### Useful commands:
`gcloud compute instances list` : List all available instances in a given group.

`gcloud compute ssh [INSTANCE NAME]` : Connect to an instance via SSH. 

## Datalab

Datalab is an environment where you can run and test Python Notebooks easily. Original documentation can be found [here](https://cloud.google.com/datalab/docs/quickstarts).

# Create Datalab
Let's use GCloud CLI for this. Open a terminal and list all the installed components with </br>
```bash
gcloud components list
```
If the `Cloud Datalab Command Line Tool` isn't installed, install it with</br>
```Bash
gcloud components install datalab
# or with
sudo apt-get install google-cloud-sdk-datalab
```
If it's installed, create an actual datalab environment with the following command:
```Bash
datalab create [VM-INSTANCE-NAME]
# For example
datalab create
```
This will create a new VM instance with the given name. You can check the deployment state on Google Cloud Console on web, or with CLI. If the deployement is done, you can reach your environment on localhost:8081. If the terminal command window used for running the datalab command is closed or interrupted, the connection to your Cloud Datalab VM will terminate, and you will need to run `datalab connect [vm-instance-name]` to reestablish the connection to your VM.

You can delete the VM with the follwoing command:
```Bash
datalab delete [vm-instance-name]
```

So we have an amazing webserver, where we can test our code, but what if we need additional packages? For exaample Keras?

Just like in a simple environment, we can use our notebook to install python packages:
```Python
!pip3 install keras
```

[Keras Install Image](https://github.com/szdani/gcloud-dl/blob/master/datalab.png?raw=true)
