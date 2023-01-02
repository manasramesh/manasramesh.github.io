---
title: Lab setup for Infra Compromise using Docker containers
date: 2022-01-02 12:30:00 +/-TTTT
categories: [Linux, Hacking, Dockers, Red Team, Log4j, Lab setup, Minecraft]
tags: [linux, hacking, dockers, redteam, log4j, lab setup, minecraft]     # TAG names should always be lowercase
---




### **Lab setup for Infra Compromise using docker containers**

  

### Introduction

In this blog I am going to share you How we can setup a infra compromise lab using docker containers. This lab was build by myself for Init_Meetup CTF. You need a Ubuntu VM to setup the lab. Follow the setps mentioned below to setup the lab.  

### Step 1 : Install Ubuntu VM

You need to install an ubuntu Virtual Machine in your computer inorder to make the infra compromise lab. We are using docker containers to setup multiple servers inside the ubuntu to make a kind of corporate infrastructure. 
[**Install Ubuntu VM**  
]https://www.geeksforgeeks.org/how-to-install-ubuntu-on-virtualbox/
Once you done installing ubuntu vm, Make the network adapter bridged or NAT and make sure the connectivity of the ubuntu vm from the attacker box.

### Step 2 : Setup docker containers.

Follow the below mentioned commands to configure and launch the docker containers. I have already uploaded all the docker images used for this lab in the dockerhub.
Open a  terminal and follow the commands.

> >sudo su

> >apt update

> >apt install docker.io

> > service docker start

> >apt install apache2

> >service apache2 start

> >docker pull manasramesh/22_s3

> >docker pull manasramesh/22_minecraft

> >docker pull manasramesh/22_ctf_rabit_hole

> >docker run -it -d -p 33:22 -p 24:21 -p 8000:8000 manasramesh/22_ctf_rabit_hole /bin/bash

> >docker run -it -d -p 25565:25565 manasramesh/22_minecraft /bin/bash

> >docker run -it -d manasramesh/22_s3 /bin/bash

Once you done executing the above commands, Start attacking the whole box from the attacker machine. None of the ubuntu ports are exposed here, so that the attack wont affect the host server.

Happy Hacking..
