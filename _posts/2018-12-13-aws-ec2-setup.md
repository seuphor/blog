---
title: AWS EC2 Python Environment Setup
date: 2018-12-13 16:57:00
categories:
- tutorials
tags:
- aws
---

Here is a setup guide for aws ec2 in python anaconda environment.

## 1. Setup EC2 Instance

Go to AWS EC2 select Ubuntu Environment. When you are in the Configure Security Group Step, please setup SSH and Custom TCP as following:

<img class="centered" src="https://i.imgur.com/2tGCM5s.png" />

Once the instance is setup, start the instance and enter into the environment with keys:
```sh
# set the key.pem to read-only by root user
chmod 400 /path/my-key-pair.pem 

ssh -i /path/my-key-pair.pem ubuntu@ec2-198-51-100-1.compute-1.amazonaws.com 
```

## 2. Install Dependencies

Here we will go through the steps to install [python-anaconda](https://repo.continuum.io/archive/) environment:
```sh
# install anaconda package
wget http://repo.continuum.io/archive/Anaconda3-4.1.1-Linux-x86_64.sh
bash Anaconda3–4.1.1-Linux-x86_64.sh

# use the updated .bashrc file
source .bashrc

# check which python you are using
which python
```

## 3. Configure Jupyter Notebook

To setup password for jupyter notebook. Type **ipython** in the terminal. Please save the 'sha' password somewhere for future usage

```python
from IPython.lib import passwd

# set up the password following the instruction
passwd()

# exit out
exit()
```

Generate a configuration file for Jupyter using (which locates at /.jupyter folder):
```sh
jupyter notebook --generate-config
```

Create certification folder for our connections in the form of .pem files
```sh
mkdir certs
cd certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

cd ~/.jupyter/
vi jupyter_notebook_config.py
```

Copy and paste following code
```sh
c = get_config()
c.IPKernelApp.pylab = 'inline'
c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False

# Your password below will be whatever you copied earlier
c.NotebookApp.password = u'sha1:941c93244463:0a28b4a9a472da0dc28e79351660964ab81605ae'
c.NotebookApp.port = 8888
```

There is also a easy way to setup, check [here](https://medium.com/@margaretmz/setting-up-aws-ec2-for-running-jupyter-notebook-on-gpu-c281231fad3f) to see the instruction.

### Up and Run

After setting up the notebook configuration file, you can run the notebook by typing:
```sh
# You should probably change the permission to /anaconda3 folder to actually run the notebook without permission error
sudo chmod -R 777 /anaconda3

nohup jupyter notebook

# https://127.0.0.1:8888
https://IPv4 Public IP:PORT
```