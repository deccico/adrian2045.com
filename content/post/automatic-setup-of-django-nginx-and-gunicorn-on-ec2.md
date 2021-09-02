+++
title = "Automatic setup of Django 1.4, NGinx and GUnicorn on EC2 using Bellatrix"
date = "2012-04-12"
tags = [
    "automation",
    "bellatrix",
    "devops",
    "django",
    "ec2",
    "gunicorn",
    "nginx",
    "jenkins",
    "snippet",
    "sysadmin",
]
featured = true
thumbnail = "/images/posts/250px-Bellatrix.jpg"
+++
![](/images/posts/jenkins_pipeline.png)

Some time ago I wrote some [lengthy instructions](/post/django-nginx-green-unicorn-in-an-ubuntu-11-10-ec2-instance) 
on how to configure Django, NGinx and Green Unicorn on a brand new Ubuntu EC2 instance. Now, I want to show how to 
automate the same tasks using a command line tool called Bellatrix. We will also provide a Jenkins (formerly Hudson) 
pipeline so we have, if we want, a nice display for the UI.


# What is Bellatrix?

![](/images/posts/250px-Bellatrix.jpg)

Bellatrix is a set of (magic) command line utilities for EC2. It wraps the boto library in order to provide a set of easy to use commands that will help you with the common EC2 operations (start, stop, list, provision, burn, etc.) You can find more information about Bellatrix on this link.

We will use Bellatrix to help us provisioning a blank Ubuntu EC2 image (AMI) by using these commands:

```bash
    pip install bellatrix
    bellatrix start ami
    bellatrix provision ubuntu_django_nginx_gunicorn.py
    bellatrix burn instance_id
    bellatrix terminate instance_id
```
 

# 1. Installing Bellatrix

First, install Bellatrix by typing:
	
```bash
pip install bellatrix
```

You can also install Bellatrix inside a virtualenv environment without modifying any of the following steps. Our 
Jenkins example (published at the end) uses virtualenv on every step.

Now that you have Bellatrix installed, you will need to provide the AWS EC2 credentials in the configuration directory. 
Please follow these instructions: 
http://bellatrix.readthedocs.org/en/latest/commands_use_tut.html#setting-your-aws-credentials in order to set your 
environment.

# 2. Starting an EC2 instance

We are ready to start an EC2 instance:

```bash
bellatrix start ami-baba68d3 ec2-keypair
```

## Parameters explanation

* ***ami-baba68d3*** is the last Ubuntu Oneiric AMI published in http://cloud.ubuntu.com/ami Just in case you don’t 
  know, an AMI (Amazon Machine Image) is just a snapshot of a pre-configured machine.
* ***ec2-keypair*** is the name of the key pair that Bellatrix (and you) will use to connect to your EC2 instance.

![](/images/posts/ubuntu_ami-150x150.png)

For more information on the bellatrix start command can be found here: 
(http://bellatrix.readthedocs.org/en/latest/commands_use_tut.html#starting-an-ec2-instance)

The above command will provide an output like this one:

```bash	
bellatrix start ami-baba68d3 ec2-keypair
+ bellatrix start ami-baba68d3 key
2012-04-01 15:06:15,848 INFO starting EC2 instance...
2012-04-01 15:06:15,848 INFO ami:ami-baba68d3 type:t1.micro key_name:key security_groups:default new size:None
2012-04-01 15:06:17,017 INFO starting image: ami-baba68d3 key key type t1.micro shutdown_behavior terminate new size None
2012-04-01 15:06:17,709 INFO we got 1 instance (should be only one).
2012-04-01 15:06:17,709 INFO tagging instance:i-864c56e2 key:Name value:Bellatrix started me
2012-04-01 15:06:21,252 INFO instance:i-864c56e2 was successfully tagged with: key:Name value:Bellatrix started me
2012-04-01 15:06:21,252 INFO getting the dns name for instance: i-864c56e2 time out is: 300 seconds...
2012-04-01 15:06:41,216 INFO DNS name for i-864c56e2 is ec2-23-20-206-220.compute-1.amazonaws.com
2012-04-01 15:06:41,217 INFO waiting until instance: i-864c56e2 is ready. Time out is: 300 seconds...
2012-04-01 15:06:41,217 INFO Instance i-864c56e2 is running
```

Please note the DNS name: ***ec2-23-20-206-220.compute-1.amazonaws.com*** and the instance id: ***i-864c56e2*** from 
the output above. We will use both in the commands below. By the way, given the DNS name we can infer the (less 
verbose) public IP. In this case is 23.20.206.220. I am sure you already got the rule.

# 3. bellatrix provision

![](/images/posts/provision-150x150.png)

Once you have your instance up and running, it’s time to configure it! Automation here is the key to a 
reliable output. Our bellatrix provision command will set-up Django, Nginx, Green Unicorn and Upstart in 
your formerly blank Ubuntu.

First, you need to get the provisioning configuration file:

```bash
wget https://bitbucket.org/deccico/bellatrix_configs/raw/tip/bellatrix_configs/ubuntu_django_nginx_gunicorn.py
```

If you are interested in getting also the Jenkins jobs for this article or looking at other provisioning examples you 
can instead clone the whole project:

```bash
hg clone ssh://hg@bitbucket.org/deccico/bellatrix_configs
```


bellatrix provision executes a list of commands. Since every command is just a simple list of Python strings it’s very easy to add your own just by looking at the available ones. Bellatrix already provides an interesting set of generic, ready to use commands that cover the most common operations.

* Bellatrix commands: – https://bitbucket.org/adeccico/bellatrix/src/tip/bellatrix/lib/cmds.py
* bellatrix provision documentation: – http://bellatrix.readthedocs.org/en/latest/commands_use_tut.html#provisioning-an-ec2-instance-or-any-host


# Command Execution

Now that you know more about the provision command, is time to execute it:

```bash
bellatrix provision ubuntu_django_nginx_gunicorn.py ubuntu 23.20.206.220 --private_key=~/.bellatrix/ec2.pk
```

## Parameters explanation

* ***ubuntu_django_nginx_gunicorn.py*** – Provisioning script. It is possible to execute a configuration inside a directory, so in case you cloned the whole project you can use  “bellatrix_configs/ubuntu_django_nginx_gunicorn.py” as this parameter.
* ***ubuntu*** – This is just the user name of the remote host.
* ***23.20.206.220*** – The host where we will execute the provisioning command. We can use the DNS name or the IP that we got in the second step.
* ***–private_key=~/.bellatrix/ec2.pk*** - The private key that correspond to the ec2-keypair specified in the start command.

 
# 4. Saving your work into a new AMI

![](/images/posts/burning-150x150.png)

Once the ***provision*** command performed its magic, we only need to save the current state into a new AMI, so every time we get a new instance from this new AMI the configuration will be ready to use. The command in this case will be:

```bash
bellatrix burn i-864c56e2  ubuntu_django_nginx_gunicorn_x64 --wait=true
```

## Parameters

* ***i-864c56e2***  -  Instance id captured in the start command.
* ***ubuntu_django_nginx_gunicorn_x64*** – Name of the new AMI. Bellatrix will add a timestamp to it.
* ***–wait=true*** – Burning a instance takes some time. By default the burn command will finish in some seconds after it gets the new AMI code. Nevertheless the AMI won’t be ready until the burning process finishes so if we specify this option, this command will return only when we get the AMI code and the burning process is done .

Documentation about the ***burn*** command: 
http://bellatrix.readthedocs.org/en/latest/commands_use_tut.html#saving-the-state-of-an-instance-into-a-new-amazon-ami



# 5. Shutting the instance down

After we get the new AMI we can safely terminate the EC2 instance with this command:

```bash	
bellatrix terminate i-864c56e2
```


As noticed [here](http://bellatrix.readthedocs.org/en/latest/commands_use_tut.html#terminating-an-ec2-instance), a 
terminated instance won’t generate any cost.

# Bonus: Adding some Jenkins magic

![](/images/posts/pipeline-150x150.png)

In case you plan to execute these commands more than once (like for any new Ubuntu release) you can find 
the Jenkins pipeline ready to be used 
[here](https://bitbucket.org/deccico/bellatrix_configs/src/d74adf3aa956/jenkins_jobs). Remember to copy the jobs to 
your Jenkins “jobs” directory and to restart Jenkins afterwards.
