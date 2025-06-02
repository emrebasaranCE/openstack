## Contents


## 1. Introduction

Hello, in this repository my main aim is to create a system on my local pc that acts as a cloud. In here openstack comes to my help actually. This tool has what you need in order to make what you want!


## 2. Installing Openstack

### 2.1. Can I Install On My Local Pc?

First we have to figure out where we can install Openstack and how to run it. [This post](https://stackoverflow.com/questions/10856551/is-it-possible-to-run-openstack-on-a-laptop-desktop) says that i can run openstack where there is a linux distribution. I wanna be sure because this post is 13 years old...

While scrolling I came up to [this article](https://ubuntu.com/openstack/install). This is official ubuntu's openstack installation article which is great for me since I am using Ubuntu.

### 2.2. I Guess I Can Install!

According to the article, lets install step by step:
```bash
sudo snap install openstack
```

After the installation, we are suggested that we can use this script:
```bash
sunbeam prepare-node-script --bootstrap
```

We can use the script like this:
```bash
sunbeam prepare-node-script --bootstrap | bash -x && newgrp snap_daemon
```

---

### 2.3. Getting Lots Of Errors...

While trying to install openstack via [ubuntu's official site](https://ubuntu.com/openstack/install), I got lots of errors and I couldn't solve them. After that I made some research and I found out that for education purposes DevStack is being used and also for beginners. For this purpose I will create a new vm ubuntu 22.04 jammy server and lots of memory(I want it to be fast). 

## 3. Changing Direction to the DevStack

I will continue with DevStack which makes more sense...

### 3.1. Creating a Vm for DevStack 

First, we have to create a VM on our Oracle VirtualBox with the image ubuntu 22.04 jammy server(To make it more faster, I won't use os's GUI).

### 3.2. Installing DevStack inside the Vm

After creating a Vm, we can now move on to the installation phase in [this link.](https://docs.openstack.org/devstack/latest/)

But before this, I want to tell what I did. To access and edit inside of the ubuntu server, I created a Port Forwarding to use ssh from the host pc. With this approach, I am more in control while using the terminal(I cannot easily use oracle virtual box's terminal. Its annoying at some point...)

---

First lets create a new user:
```bash
sudo useradd -s /bin/bash -d /opt/stack -m stack
```

After that, we can give executable permissions to our newly created user:
```bash
sudo chmod +x /opt/stack
```

We also need sudo privileges:
```bash
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo -u stack -i
```

And then lets download DevStack:
```bash
git clone https://opendev.org/openstack/devstack
cd devstack
```

---

After this step, to control my terminal, I added this configuration:
```bash
nano ~/.inputrc
"\C-h": backward-kill-word
```
This command lets you delete words with ctrl + backspace which is needed while working in a terminal environment!

---

After downloading the DevStack, we can create a local.conf file with the contents like this:
```conf
[[local|localrc]]
ADMIN_PASSWORD=secret # I changed this value of course...
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```

After this step, we can start to install:
```bash 
./stack.sh
```

This command download and installs lots of packages, therefor it will take time to download based on your internet connection.

```output
2025-06-02 11:53:56.737 | stack.sh completed in 1693 seconds.
```
If you managed to see this output, congratulations! You successfully installed DevStack to you server or hardware!

## 4. Looking Inside Our **Cloud**

Lets go to the openstack dashboard and try to gain some knowledge:

![alt text](images/image.png)

From the image we can see that our cloud is up and running which is great!

### 4.1. Lets Upload Ubuntu Image to Our Cloud!

First intall image to the Vm like this:
```bash
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```

To get access for openstack commands, we can use this command:
```bash
source ~/devstack/openrc admin admin
```

And uplaod image to the `Glance` service:
```bash
openstack image create "ubuntu-22.04" \
  --file jammy-server-cloudimg-amd64.img \
  --disk-format qcow2 \
  --container-format bare \
  --public
```

```bash
### NOTE: This is the same command but with commands
# image name to show on openstack
openstack image create "ubuntu-22.04" \ 
  # file path \
  --file jammy-server-cloudimg-amd64.img \
#   file format of the disk image \
  --disk-format qcow2 \ 
  # even tho i loooked for this, i have no idead what is does... \
  --container-format bare \
  # makes available to the all users using Openstack \
  --public 
```

If you got this kind of output then its uploaded to our local cloud:
![alt text](images/image-2.png)

If we look at the dashboard, we can also see that our image has arrived there too:
![alt text](images/image-3.png)
Note: The first image is DevStack's default image. It's a tiny Linux distribution (CirrOS) used to test the basic functionality of OpenStack.

### 4.2. Creating a New Instance

Lets create a new instance with our newly uploaded iamge! 