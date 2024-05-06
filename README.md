# Active Directory Methodology for OSCP and Beyond

## Introduction

Hi, I'm Yash Urade. I recently passed the OSCP exam this April, and I'm excited to share my methodology for Active Directory. Active Directory (AD) is the backbone of many enterprise networks, and understanding its intricacies is crucial for any cybersecurity professional. This my methodology and how I started with Active Directory, I'll walk you through the fundamentals of Active Directory, techniques for enumeration, exploitation, post-exploitation, Attacks to remember and some defense stuff. Let's dive in!

## Active Directory Fundamentals

## What is Active Directory?

Active Directory is a centralized database used for managing users, passwords, computers, servers, and other resources across a network. Key components include:

- **Domains**: Logical groupings of network objects with a common security boundary.
  
- **Forests**: Collections of one or more domains sharing a common schema, configuration, and global catalog.

- **Domain Controllers**: Servers responsible for authenticating users and maintaining the AD database.

- **LDAP (Lightweight Directory Access Protocol)**: Protocol for accessing and managing directory data.

- **Kerberos**: Network authentication protocol providing secure authentication between clients and servers. For a detailed explanation of Kerberos, check out this video: [Understanding Kerberos Authentication](https://youtu.be/snGeZlDQL2Q?si=JDWBcWTB-Z-qN4CD) by vbscrub.

- **SMB (Server Message Block)**: Network file sharing protocol used for accessing shared resources.


## Enumeration

Enumeration is the process of gathering information about an Active Directory environment. This phase is crucial for understanding the network topology, identifying potential attack vectors, and planning the next steps of the engagement. We'll explore tools and techniques for enumerating domains, domain controllers, users, groups, and computers.
Enumeration is the process of gathering information about an Active Directory (AD) environment, and it's a crucial step in penetration testing. Here are some steps and tools you can use for effective enumeration:

  # 1. Identifying Active Directory Machines

    - **Nmap Scans**: Use Nmap to scan the network and identify machines running Active Directory services. Look for open ports such as 389 (LDAP), 88 (kerberos),445 (SMB), 5985 & 5986 (winrm http) and 135 (RPC). The domain: htb.local point towards AD.

      Example command:
      ```bash
        ┌──(kali㉿kali)-[~/HTB]
          └─$ cat nmap.txt 
          # Nmap 7.94SVN scan initiated Sun May  5 19:57:18 2024 as: nmap -A -p- -T4 -oN nmap.txt 10.10.10.161
          Nmap scan report for 10.10.10.161
          Host is up (0.087s latency).
          Not shown: 65513 closed tcp ports (conn-refused)
          PORT      STATE SERVICE      VERSION
          88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2024-05-06 00:06:08Z)
          135/tcp   open  msrpc        Microsoft Windows RPC
          139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
          389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
          445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
          464/tcp   open  kpasswd5?
          593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
          636/tcp   open  tcpwrapped
          3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
          3269/tcp  open  tcpwrapped
          5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
          |_http-server-header: Microsoft-HTTPAPI/2.0
          |_http-title: Not Found
          9389/tcp  open  mc-nmf       .NET Message Framing
          47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
          |_http-title: Not Found
          |_http-server-header: Microsoft-HTTPAPI/2.0
          49664/tcp open  msrpc        Microsoft Windows RPC
          49665/tcp open  msrpc        Microsoft Windows RPC
          49666/tcp open  msrpc        Microsoft Windows RPC
          49668/tcp open  msrpc        Microsoft Windows RPC
          49671/tcp open  msrpc        Microsoft Windows RPC
          49676/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
          49677/tcp open  msrpc        Microsoft Windows RPC
          49682/tcp open  msrpc        Microsoft Windows RPC
          49701/tcp open  msrpc        Microsoft Windows RPC
          Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows
          
          Host script results:
          | smb2-time: 
          |   date: 2024-05-06T00:07:05
          |_  start_date: 2024-05-06T00:02:42
          | smb-security-mode: 
          |   account_used: guest
          |   authentication_level: user
          |   challenge_response: supported
          |_  message_signing: required
          | smb-os-discovery: 
          |   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
          |   Computer name: FOREST
          |   NetBIOS computer name: FOREST\x00
          |   Domain name: htb.local
          |   Forest name: htb.local
          |   FQDN: FOREST.htb.local
          |_  System time: 2024-05-05T17:07:03-07:00
          | smb2-security-mode: 
          |   3:1:1: 
          |_    Message signing enabled and required
          |_clock-skew: mean: 2h26m47s, deviation: 4h02m31s, median: 6m46s
          
          Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
          # Nmap done at Sun May  5 20:00:26 2024 -- 1 IP address (1 host up) scanned in 187.96 seconds

      ```

  # 2. Enumerating Active Directory

  Once you've identified AD machines, you can use various tools to enumerate AD objects and gather valuable information:

  - **smbmap**: Use smbmap to enumerate shares, users, and groups on SMB-enabled machines. This can provide insights into the structure and permissions of the AD environment.

  - **smbclient**: smbclient is another tool for interacting with SMB shares on remote machines. You can use it to list shares, download/upload files, and gather information about the system.

  - **rpcclient(enumdomusers)**: rpcclient can be used to enumerate domain users by querying the domain controller. Use the `enumdomusers` command to list all users in the domain.  

  - **ldapsearch**: ldapsearch is a command-line tool for querying LDAP servers. Use it to search for naming contexts and retrieve information about the LDAP directory structure.

  Example command:
  ```bash
  ldapsearch -H ldap://ip -x -s  base namingcontexts
  ```


#################STill in work


## Exploitation

With information gathered during the enumeration phase, we can now identify vulnerabilities and weaknesses within the Active Directory environment. This includes exploiting misconfigured permissions, weak passwords, vulnerable services, and other common issues. We'll discuss various exploitation techniques and how to escalate privileges within an Active Directory environment.

## Post-Exploitation

Once we've gained initial access to the Active Directory environment, our goal is to maintain access, move laterally within the network, and establish persistence. Post-exploitation techniques such as pass-the-hash, pass-the-ticket, and lateral movement will be covered in this section. We'll also discuss methods for evading detection and covering our tracks.

## Defense and Mitigation

Finally, we'll explore best practices for defending Active Directory environments against attacks. This includes implementing least privilege, strong authentication mechanisms, and proper segmentation. We'll also discuss techniques for detecting and responding to Active Directory attacks, as well as tools that can aid in this process.

## Conclusion

In conclusion, understanding Active Directory is essential for any cybersecurity professional, whether you're preparing for the OSCP exam or seeking to enhance your skills in AD security. By following the methodology outlined in this guide, you'll gain the knowledge and techniques needed to navigate and secure Active Directory environments effectively.

