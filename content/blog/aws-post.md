+++
title = "Have Jupyter running on AWS even when you're not there!"
date = "2017-04-22T13:07:31+02:00"
tags = ["AWS", "Jupyter", "Python"]
categories = ["Tutorial"]
banner = "img/banners/pic5.png"
+++

## Setup

Setting up a Jupyter notebook on an AWS ec2 instance to be able to run even when you close your laptop has to be one of the most useful tools ever! However, there's not enough information online on how to do it.

From bits and pieces from all over the internet, here's how you do it step by step..

**Step 1: Setting up SSH** 
Make sure you have an ssh key first.  You can do that by typing in the command line:
 ```commandline
ls ~/.ssh/*id_rsa*
```
If there is not a file there, generate an ssh key by typing 
```commandline
ssh-keygen -t rsa
```
When asked where to save, save it in:     
`/Users/username/.ssh/id_rsa`


 **Step 2: Setting up AWS Account**
 Sign up for an AWS account if you don't have one. If you do, sign in and click on EC2 under the compute tab.
 
*  Next click on `Network & Security`
*  Click on `Key Pairs`
*  Click on `Import key pair`
* Give it a name `id_rsa`
 
 Go on your terminal and type `cat ~/.ssh/id_rsa.pub`
 
 Copy and paste output into the wndow and press Import
 
 **Step 3: Create Instance**
 * Go to Instances. Choose a server(I chose Ubuntu Server)
 
 * Choose an instance type(Choose t2.micro instance because it's free for a year)
 
 * Click Next:Configure Instance Details:    
 
 * Click Next: Add Storage
 
 * Change Size (GIB) from 8 to 30 and keep the rest of the fields the same.
 
 * Click Next: Add Tags
 
 * Click Next: Configure Security Group
 
 * Change or keep network to look like this(You can change your permissions later):


 ![awspermis](/img/aws_permis.png)

 
 *  Click Review and Launch. Choose an existing key pair. Select id_rsa and check the I acknowledge box.
 
 **Step 4: Logging on Instance**
 
 * Go to Instances and click once on your instance. On the bottom right, there should be a field that said `IPv4 Public IP:` Copy the IP Address to the right of the field and in your terminal type:
  
  `ssh ubuntu@123.123.123.123`    
  
  The 123.123.123.123 being the IPv4 Public IP that you copied
  
  * You may see a message that starts with "The Authenticity of host..." Type `Yes`
  
 **Step 5: Installing Python and Packages**
 
* Update Libraries
```commandline
sudo apt-get update
```

* Install Python 3
```commandline
sudo apt-get install python3 python3-pip
```

* Install Git
```commandline
sudo apt-get install git
```
* Install Anaconda
```commandline
wget https://repo.continuum.io/archive/Anaconda3-4.3.1-Linux-x86_64.sh
```
```commandline
sudo bash Anaconda3-4.3.1-Linux-x86_64.sh
```

```commandline
export PATH=~/anaconda3/bin:$PATH
```

```commandline
conda update conda
```

* Fix Permissions if it doesn't work
```commandline
sudo chown -R ubuntu /home/ubuntu/anaconda3 
sudo chmod -R +x /home/ubuntu/anaconda3
```

* Install Jupyter
```commandline
conda install jupyter
```

* Install Tmux  (This allows Jupyter to keep on running)
```commandline
sudo apt-get install -y tmux
```

**Step 6: Running Jupyter**

* Create a new tmux instance that runs all the time

* This Command shows active terminals
```commandline
tmux ls
```

* Create new terminal(I called mine jup)
```commandline
tmux new -s jup
```

* Type in the tmux screen:
```commandline
jupyter notebook --no-browser
```
* DONT CLOSE YOUR AWS TERMINAL
Now on your local computer open up a new terminal and type in (ip is your public Ipv4 from before):

```commandline
ssh -NL 8157:localhost:8888 ubuntu@123.123.123
```

* On your local computer go on a web browser and type in localhost:8157

* Copy and paste your token from your ec2 instance into the password or token field. Only copy after the "token="
It should look something like this 
```commandline
http://localhost:8888/?token=349hg3894gh3489gj398ggg45g
```

* If you're on a mac on the ec2 instance, type control + b d to detach from the tmux. You might have to practice a couple times. I usual hold control and b. Quickly let go and click d. Never type exit on a tmux screen otherwise you will lose your running jupyter notebook. You may close your aws instance now by typing exit after you've detached from your tmux.


Now you can close your your laptop, have your computations running, go wherever and your jupyter notebook will be running. If you ever want to access your notebook you don't have to login to your instance. All you have to do type:
```commandline
ssh -NL 8157:localhost:8888 ubuntu@123.123.123
```
on your local terminal. Go on localhost:8157 in a browser and your jupyter notebooks will be there!
