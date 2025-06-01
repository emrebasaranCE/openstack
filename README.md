## Contents


## 1. Introduction

Hello, in this repository my main aim is to create a system on my local pc that acts as a cloud. In here openstack comes to my help actually. This tool has what you need in order to make what you want!


## 2. Installing Openstack

## 2.1. Can I Install On My Local Pc?

First we have to figure out where we can install Openstack and how to run it. [This post](https://stackoverflow.com/questions/10856551/is-it-possible-to-run-openstack-on-a-laptop-desktop) says that i can run openstack where there is a linux distribution. I wanna be sure because this post is 13 years old...

While scrolling I came up to [this article](https://ubuntu.com/openstack/install). This is official ubuntu's openstack installation article which is great for me since I am using Ubuntu.

## 2.2. I Guess I Can Install!

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

After the installation is completed, we can now move on to the next stage:
```bash
sunbeam cluster bootstrap --accept-defaults
```
