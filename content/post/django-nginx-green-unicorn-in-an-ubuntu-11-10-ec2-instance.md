+++
title = "Setting up Django 1.3 + NGinx 1.0.5 + Green Unicorn 0.13 in an Ubuntu 11.10 EC2 instance"
date = "2011-12-27"
tags = [
    "python",
    "django",
    "ec2",
    "gunicorn",
    "nginx",
    "snippet",
    "sysadmin",
    "ubuntu",
]
featured = true
+++

Django has become the de-facto web framework for Python. Although, since Django just specializes in dynamic content, 
you have to combine it (at least in production) with an HTTP server to serve static content such as css, javascript 
files and images files. In the past, the communication protocol between Python web applications was CGI, FastCGI or 
mod_python. But after PEP-333 was accepted, the faster and more efficient WSGI became the standard.

Green Unicorn is a Python WSGI HTTP Server for UNIX. Its combination with the high performance HTTP server Nginx is 
gaining a lot of momentum in the Python community.

From “man gunicorn”…

>    Green Unicorn (gunicorn) is an HTTP/WSGI server designed to serve
>    fast clients or sleepy applications. That is to say; behind a
>    buffering front-end server such as nginx or lighttpd.


We are describing here how to combine a Django application with Green Unicorn and Nginx within a pristine EC2 
Ubuntu 11.10 image.

# Getting an Ubuntu instance

![](/images/posts/ami_locator211-300x168.png)

First we need to go to http://cloud.ubuntu.com/ami/ to check the last released ami. As described in the image, we are 
looking for the last 64 bits, Ubuntu 11.10 EB2 image in the US-East region, which so far is ami-bf62a9d6.

* Why Ubuntu 11.10 ? Because is the last stable release.
* Why EBS? Because is faster and with more options like on the fly snapshots or easy image resizing than instance-store.
* Why US-EAST? Because is cheaper and usually with more features than other regions.
* We also choose 64 bits so we can eventually expand to a bigger instance.

# Starting your free EC2 instance

So far there is a nice offer for new customers where you can get a free EC2 Linux Micro Instance usage (613 MB of 
memory and 32-bit and 64-bit platform support) for one year. Please open your account and then launch your instance as 
described in the following screenshots:

<img src="/images/posts/00-launching-instance.png" width="100%"/>
<img src="/images/posts/01-introducing-Ubuntu-ami.png" width="100%"/>
<img src="/images/posts/02-availability-zone.png" width="100%"/>
<img src="/images/posts/03-name-tag.png" width="100%"/>
<img src="/images/posts/04-creating-a-new-key-pair.png" width="100%"/>
<img src="/images/posts/05-enabling-incoming-SSH-and-HTTP.png" width="100%"/>
<img src="/images/posts/06-review.png" width="100%"/>
<img src="/images/posts/07-instance-started.png" width="100%"/>



# Launching and configuring your Ubuntu box

At this point you should have your EC2 or your own Ubuntu box up and running. If you are using EC2 you should ssh into 
it with this command:

```bash
ssh -i you_private_key.pem ubuntu@DNS_NAME
```

Then, please follow this steps to start setting your environment:

```bash
#install Python Package Installer
sudo apt-get install python-pip
#upgrade PIP itself
sudo pip install pip --upgrade
#install Green Unicorn
#install Virtualenv to generate our own isolated environment
sudo pip install virtualenv
#Installing NGINX
sudo apt-get install nginx
#Creating our Virtualenv environment
virtualenv --no-site-packages django_app
cd django_app
#activating the environment
source bin/activate
#installing Green Unicorn and Django
pip install django gunicorn
#Creating a Django project
django-admin.py startproject app
cd app
#start Django application with Green Unicorn
gunicorn_django -b 0.0.0.0:8000
```

Now please go to your browser and check if your application is working as expected:

![](/images/posts/testing-django-application.png)

Finally, just Ctrl+C and “deactivate”, so you can continue with Nginx…


# Setting up Nginx

Now we will set up Nginx in order to have it listening in the port 80, serving static files and working with Gunicorn and Django in order to serve dynamic content.

```bash
#prepare directories
sudo mkdir -p /opt/django/logs/nginx/
#Create directories and softlinks for static content and templates
mkdir $HOME/django_app/static
mkdir $HOME/django_app/templates
sudo ln -s $HOME/django_app/static /opt/django
#download and set up Nginx configuration. Basically it will listen in port 80 and forward to port 8000 dynamic content
sudo mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.backup
wget https://bitbucket.org/deccico/django_gunicorn/raw/tip/server/etc/nginx/sites-available/default
sudo cp default /etc/nginx/sites-available/default
```

 
# Testing Static resources

## Configuring settings.py

You need to tell Django where your templates are, for that reason, add your absolute “templates” directory to your /home/ubuntu/django_app/app/settings.py file

Now look for the TEMPLATE_DIRS section and add your template directory

```bash
TEMPLATE_DIRS = (
# Put strings here, like "/home/html/django_templates" or "C:/www/django/templates".
# Always use forward slashes, even on Windows.
# Don't forget to use absolute paths, not relative paths.
'/home/ubuntu/django_app/templates',
)
```


## Configuring urls.py

/home/ubuntu/django_app/app/urls.py will help us telling Django to return our template page when we introduce the /test_static request.

To make things easier we are using a dynamic view:

```bash
from django.conf.urls.defaults import patterns
from django.views.generic.simple import direct_to_template
 
urlpatterns = patterns('',
(r'^test_static/$',             direct_to_template, {'template': 'test_static.html'}),
)
```

## Preparing test template

We need to prepare our template. Please create this file /home/ubuntu/django_app/templates/test_static.html with the following content:

```html
<!--Load static will recover the static prefix variable and saving it in STATIC_PREFIX. We do this only once.-->
 
{% load static %}
 
{% get_static_prefix as STATIC_PREFIX %}
 
<!--Here we are just showing how easy is to recover static resources . -->
Test image
<img src="{{ STATIC_PREFIX }}django.png" />
```


After creating the template file you will need an actual image :) You can download one from the same repo.

```bash
cd /home/ubuntu/django_app/static
 
wget https://bitbucket.org/deccico/django_gunicorn/raw/tip/static/django.png
```


# Restarting Nginx and voilà


Finally, we just need to restart Nginx and test everything is working.

```bash	
#First we restart Nginx
 
sudo service nginx restart
```

Now we will start our Django application again:

```bash	
cd /home/ubuntu/django_app/app
#activating the environment
source ../bin/activate
#start Django application with Green Unicorn
gunicorn_django -b 0.0.0.0:8000
```

Then we just go to our browser and request test_static in port 80. In my case the full address is: 
http://ec2-107-20-88-133.compute-1.amazonaws.com/test_static

![](/images/posts/testing-static.png)

As you can see above, we got Nginx serving the image while Django give us the content.

Type “Ctrl + C” and then “deactivate” again so we can be ready for the next step…

 
# Setting Green Unicorn automatic start

We will use ***Upstart*** so we don’t have to worry about starting our application whenever we need to restart the 
server. One more configuration file and a bootstrap script will do the job here:

In /home/ubuntu/django_app/run.sh we create:

```bash	
#!/bin/bash
set -e
LOGFILE=/var/log/gunicorn/django_app.log
LOGDIR=$(dirname $LOGFILE)
NUM_WORKERS=3  #recommended formula here is 1 + 2 * NUM_CORES
 
#we don't want to run this as root..
USER=www-data
GROUP=www-data
 
cd /home/ubuntu/django_app
source bin/activate
cd app
test -d $LOGDIR || mkdir -p $LOGDIR
exec gunicorn_django -w $NUM_WORKERS 
  --log-level=debug 
  --log-file=$LOGFILE 2>>$LOGFILE 
  --user=$USER --group=$GROUP
```

After creating the file, please remember to execute chmod a+x /home/ubuntu/django_app/run.sh

On the other hand, Upstart configuration file have to be located in: /etc/init/django_app.conf with the following 
content:

```bash	
description "Django Application"
 
start on runlevel [2345]
 
stop on runlevel [06]
 
respawn
 
respawn limit 10 5
 
exec /home/ubuntu/django_app/run.sh
```

Now we can control our application with:

```bash
sudo start django_app
 
sudo stop django_app
```


# To sum up

The purpose of this post is to provide a path to make the installation and configuration of this stack easier. While 
some things could be changed or improved the focus was to stress how easily we can configure everything from scratch 
rather than exploring or discussing different options.

The code and configuration files of this post can be found here: https://bitbucket.org/deccico/django_gunicorn Have fun!

 
# References

* https://docs.djangoproject.com/en/1.3/ref/generic-views/
* http://upstart.ubuntu.com/getting-started.html
* http://senko.net/en/django-nginx-gunicorn/
* https://help.ubuntu.com/community/EC2StartersGuide
* https://help.ubuntu.com/community/UEC/Images
* https://help.ubuntu.com/community/UbuntuCloudInfrastructure
* http://cloud.ubuntu.com/ami/



Update: these instructions are now automated in this new post

Note: This article was originally published here: http://adrian.org.ar/django-nginx-green-unicorn-in-an-ubuntu-11-10-ec2-instance/
you can find the old post here: https://web.archive.org/web/20140527123847/http://adrian.org.ar/django-nginx-green-unicorn-in-an-ubuntu-11-10-ec2-instance/

Here are some comments received for the post:

![](/images/posts/django_nginx_gunicorn_comments.png)
a
![](/images/posts/django_nginx_gunicorn_comments2.png)