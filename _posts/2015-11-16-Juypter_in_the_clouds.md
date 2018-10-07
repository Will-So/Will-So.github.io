---
layout: post
title: "Jupyter in the Clouds"
draft: false
tags: [AWS, UNix]
---

Using the Jupyter Notebook with an EC2 instance allows us to analyze data of a size that would be difficult or impossible to do on our local machines. The most powerful (from a memory perspective) EC2 instance has 244GB of Ram and 32 cores and often costs only 50 cents/hr. 

This tutorial explains:

1. How to Set up the Jupyter Notebook on an EC2 instance so we can use it from a web browser.
2. How to keep the notebook running even if the SSH session closes.
3. A workflow for dealing with spot instances. 

This post assumes you are already familiar with the basics of EC2. You should know how to start an instance and SSH into it. 

# Step 1: Setting up your EC2 Instance

1. Go to the EC2 Management Console and select launch instances
2. Choose which AMI you want to use. I recommend Ubuntu. There might be some minor differences if you choose something else. [^1]
3. Select which type of instance you want and change any additional settings
4. In the security group page, enable HTTPS and Port 9999. Port 9999 is the port we will be accessing the notebook from:
![](https://s3.us-east-2.amazonaws.com/screens12/Screens/S3570.png)
3. Connect to your EC2 Instance with SSH
4. Install all the software you need including the Notebook[^3]
    - The link to install conda can be found [here](https://www.continuum.io/downloads). Just copy the link address for linux and then use `wget link` on your remote host to download it. Just follow the directions on the page. The installation will ask whether to append the conda environment to the Python path. Type in `yes`
    - After installing, you need to reload `.bashrc` (type `. .bashrc`)

# Step 2: Configuring the Notebook Server

The Jupyter Documentation provides an excellent walkthrough on configuring the server [here](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html). The tl;dr is:

1. In the terminal type: `jupyter notebook --generate-config` in order to generate the configuration file.
2. In IPython type in `from notebook.auth import passwd; passwd()` and then enter in the password you want to use to access the notebook. Remember the password and copy the hash you get back. 
3. Generate an SSL certificate using the following code:
    
        openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.key -out mycert.pem
        
4. Open the configuration file (`~/.jupyter/jupyter_notebook_config.py` by default) you made with your favorite text editor and copy in the hash generated from passwd():

    ```python
c = get_config()
c.NotebookApp.password = u'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 9999
c.NotebookApp.certfile = u'/home/ubuntu/mycert.pem'
c.NotebookApp.keyfile = u'/home/ubuntu/mycert.key'

    ```

5. In `.bashrc` make the following alias:

    `alias jupyter='nohup jupyter notebook &'`
    
Then reload `.bashrc` again. This also makes your jupyter session persistent. It will not stop running just because the ssh session is closed.

Now you can type `jupyter` in the terminal and the server should run. Then browse to https://ip_address:9999 and login with the password you entered:

<img src="https://s3.us-east-2.amazonaws.com/screens12/S3574.png" width="800"/>




# Protecting your data while using Spot Instances 

Spot instances are much cheaper than on-demand instances. They downside is that you can be kicked off your instance if the amount of capacity of the Spot Instance gets too high. 

The best way to deal with the transient nature of spot instances is to use the following workflow:

1. Write the code on your local-host
2. Prototype the code on your local-host with a small sample of the data you want to process.
3. Move your code to your EC2 instance and then run the code. I usually use github for this. 

This way we minimize the amount of time that we have to run the EC2 instance and we don't have to worry about losing any code because it is all in the local host.

If you end up writing novel code in your EC2 instance I recommend keeping your code synced with github. 

# Footnotes

[1]: Some commands might be different above if you choose a different AMI. In the future, I recommend creating your own AMI so you don't have to do this every time.

[3]: For installing the apps, I recommend you use [Anaconda](https://www.continuum.io/downloads) to install Python and dependencies. Note that you will have to restart your shell session in order to use 
