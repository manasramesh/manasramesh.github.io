---
title: Lab setup for Infra Compromise using Docker containers
date: 2022-01-02 12:30:00 +/-TTTT
categories: [Linux, Hacking, Dockers, Red Team, Log4j, Lab setup, Minecraft]
tags: [linux, hacking, dockers, redteam, log4j, lab setup, minecraft]     # TAG names should always be lowercase
---




### **Lab setup for Infra Compromise using docker containers**

  

### Introduction

In this blog I am going to share you How we can setup a infra compromise lab using docker containers. This lab was build by myself for Init_Meetup CTF. You need a Ubuntu VM to setup the lab. Follow the setps mentioned below to setup the lab.  

### Step 1: Install Ubuntu VM
Download Ubuntu Server from https://ubuntu.com/download/server

### Step 2: Install Docker
```bash
sudo apt-get update
sudo apt-get install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

### Step 3: Pull Docker Images
```bash
docker pull vulhub/nginx:1.19.0
docker pull vulhub/php:7.4
docker pull vulhub/mysql:5.7
```

### Step 4: Create Docker Network
```bash
docker network create redteam-lab
```

### Step 5: Run Containers
```bash
docker run -d --name nginx --network redteam-lab -p 80:80 vulhub/nginx:1.19.0
docker run -d --name php --network redteam-lab vulhub/php:7.4
docker run -d --name mysql --network redteam-lab -e MYSQL_ROOT_PASSWORD=redteam vulhub/mysql:5.7
```

Once you done executing the above commands, Start attacking the whole box from the attacker machine. None of the ubuntu ports are exposed here, so that the attack wont affect the host server.

Happy Hacking..
