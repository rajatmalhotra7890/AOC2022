[Day 4] Scanning Scanning through the snow

- During the investigation of the downloaded GitHub repo (OSINT task), elf Recon McRed identified a URL `qa.santagift.shop` that is probably used by all the elves with admin privileges to add or delete gifts on the Santa website. 
- The website has been pulled down for maintenance, and now Recon McRed is scanning the server to see how it's been compromised. Can you help McRed scan the network and find the reason for the website compromise?

**Learning Objectives**

-   What is Scanning?
-   Scanning types
-   Scanning techniques
-   Scanning tools

## What is Scanning?
- It is a set of procedures for identifying live hosts, ports, and services discovering the OS of the target system and even identifying threats and vulnerabilities.
- These scans are typically automated and give an insight into what could be exploited.
- Scanning could increase an attack surface for the attacker.

#### Scannning Types
1. Passive Scanning
- This method involves scanning a network without directly interacting with the target device (server, computer etc.). Passive scanning is usually carried out through packet capture and analysis tools like Wireshark; however, this technique only provides basic asset information like OS version, network protocol etc., against the target.

2. Active Scanning
- This is the type of scanning where you can scan individual endpoints in an IT network to retrieve more detailed info.
- This is used to get detailed info about the target.


### DIY Task - Read about Nmap and Nikto here
[Blog for Nikto](https://null-byte.wonderhowto.com/how-to/scan-for-vulnerabilities-any-website-using-nikto-0151729/)
[Nikto Cheatsheet](obsidian://open?vault=AOC22&file=Day%204%2FNIkto-Cheat-Sheet.pdf)

[Blog for Nmap](https://hackertarget.com/nmap-tutorial/)
[Nmap Cheatsheet](https://www.stationx.net/nmap-cheat-sheet/)


- Elf Recon McRed ran Nmap and Nikto tools against the QA server to find the list of open ports and vulnerabilities. He noticed a Samba service running - hackers can gain access to the system through loosely protected Samba share folders that are not protected over the network. He knows that The Bandit Yeti APT got a few lists of admin usernames and passwords for `qa.santagift.shop` using OSINT techniques.

Nmap Scans
```
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:BD:91:8E:42:FB (Unknown)
```

- Now we use smbclient/file explorer to list the shares as well
![](../img/Pasted%20image%2020221205072412.png)
`There is a peculiar share called "admins"`

##### Step 1 - Use this command from file explorer to access the share!
![](../img/Pasted%20image%2020221205073530.png)

##### Step 2 - Use the credentials (ubuntu:S@nta2022) found to access the share!
![](../img/Pasted%20image%2020221205073643.png)

Here we see that the folder `admins` on the share has two files 
![](../img/Pasted%20image%2020221205073712.png)

Contents of flag.txt
```
{THM_SANTA_SMB_SERVER}
```

Contents of userlist.txt
```
USERNAME	PASSWORD
santa		santa101
santahr		santa25
santaciso	santa30
santatech	santa200
santaaccounts	santa400
```

## Questions
1. What is the name of the HTTP server running on the remote host?
Answer - `Apache`
![](../img/Pasted%20image%2020221205073300.png)

2. What is the name of the service running on port 22 on the QA server?
Answer - `ssh`
![](../img/Pasted%20image%2020221205073349.png)

3. What flag can you find after successfully accessing the Samba service?
Answer - `{THM_SANTA_SMB_SERVER}` 

As see earlier from the contents

4. What is the password for the username santahr?
Answer - `santa25`

As see from contents on `userlist.txt`

Day 4 Done!!
