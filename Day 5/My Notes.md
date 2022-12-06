[Day 5] Brute-Forcing He knows when you're awake

- Elf McSkidy asked Elf Recon McRed to search for any backdoor that the Bandit Yeti APT might have installed. If any such backdoor is found, we would learn that the bad guys might be using it to access systems on Santa’s network.

Learning Objectives

    Learn about common remote access services.
    Recognize a listening VNC port in a port scan.
    Use a tool to find the VNC server’s password.
    Connect to the VNC server using a VNC client.

### What are remote access services?
- You can easily control your computer system using the attached keyboard and mouse when you're at your computer.
- However if we need to perform remote administration on a computer system which is in a different country, room or place in the world, we need different types of software packages and tools built with specific protocols to accomplish this

Some of the common Remote Access Services are:
1. SSH
2. RDP
3. VNC

#### What is VNC?
- It is called Virtual Network Computing 
- It provides access to a GUI which allows a user to view the desktop and (optionally) control the mouse and keyboard.
- VNC is available for any system with a GUI, including MS Windows, Linux, Android, MacOS or even Raspberry Pi

We could select one of these tools according to our needs, to control a system remotely.
- However, we do need to authenticated ourselves to the remote server.


## Primer about Authentication
- Authentication is the process of where a system validates your identity.
- The process goes the following way:
1. The user claims an identity, or a username
2. Furthermore, user needs to prove their identity.
- This process is achieved by one of the following 
		1. Something you know refers, in general, to something you can memorize, such as a password or a PIN (Personal Identification Number).
	    2. Something you have refers to something you own, hardware or software, such as a security token, a mobile phone, or a key file. The security token is a physical device that displays a number that changes periodically.
	 3. Something you are refers to biometric authentication, such as when using a fingerprint reader or a retina scan.

Back to remote access services, we usually use passwords or private key files for authentication. Using a password is the default method for authentication and requires the least amount of steps to set up. Unfortunately, passwords are prone to a myriad of attacks.


### Attacking Passwords
- Passwords are the most commonly used authentication method.
- Unfortunately, they are exposed to a myraid of attacks

We could use automated tools for password attacks or manually guess passwords too.

Some of the ways used in attacks against passwords are:
1. `Shoulder Surfing` 
- Looking over the victim’s shoulder might reveal the pattern they use to unlock their phone or the PIN code to use the ATM. This attack requires the least technical knowledge.

2. `Password guessing`
- Without proper cyber security awareness, some users might be inclined to use personal details, such as birth date or daughter’s name, as these are easiest to remember. Guessing the password of such users requires some knowledge of the target’s personal details; their birth year might end up as their ATM PIN code.

3. `Dictionary Attack`
- This approach expands on password guessing and attempts to include valid words in a dictionary or a word list.

4. `Brute-Force Attack`
- This approach is the most exhaustive and time-consuming
- This is where the attacker needs to guess passwords and try all different kinds of combinations against the password

Let’s focus on dictionary attacks. Over time, hackers have compiled one list after another of passwords leaked from data breaches. One example is RockYou’s list of breached passwords, which you can find on the AttackBox at /usr/share/wordlists/rockyou.txt. The choice of the word list should depend on your knowledge of the target. For instance, a French user might use a French word instead of an English one. Consequently, a French word list might be more promising.

RockYou’s word list contains more than 14 million unique passwords. Even if we want to try the top 5%, that’s still more than half a million. We need to find an automated way.


### Hacking an Authentication Service
- We want an automated way to try the common passwords or the entries from a word list; here comes THC Hydra. Hydra supports many protocols, including SSH, VNC, FTP, POP3, IMAP, SMTP, and all methods related to HTTP. You can learn more about THC Hydra by joining the Hydra room. The general command-line syntax is the following:
```
hydra -l login_username -P password_list.txt <server-ip> <service>
```
- `-l denotes the login name of the target/username which we need to log in to as`
- `-P - password list name on local system`
- `server-ip - IP address of target`
- `service - indicates the service which you want to launch a dictionary attack against`

You can replace ssh with another protocol name, such as rdp, vnc, ftp, pop3 or any other protocol supported by Hydra.


Nmap Results for the target
```
PORT     STATE SERVICE
22/tcp   open  ssh
5900/tcp open  vnc
```

Note - This specific VNC server doesnt use a username 

Hydra Brute-force results against the VNC service 
```
hydra -P password.txt <server-ip> vnc -vV
```

![](../img/Pasted%20image%2020221206103631.png)
The password is `1q2w3e4r`

We then use Remmina (a remote access service client) to access the VNC server
![](../img/Pasted%20image%2020221206103757.png)

Hence we login to the VNC server and get the flag
![](../img/Pasted%20image%2020221206103826.png)


## Questions
1. Use Hydra to find the VNC password of the target with IP address 10.10.87.235. What is the password?
Answer - `1q2w3e4r`
`As seen above in Hydra Results` 

2. Using a VNC client on the AttackBox, connect to the target of IP address 10.10.87.235. What is the flag written on the target’s screen? 
Answer - `THM{I_SEE_YOUR_SCREEN}`
`As see above in the Screenshot`

3. If you liked the topics presented in this task, check out these rooms next: Protocols and Servers 2, Hydra, Password Attacks, John the Ripper. 


Day 5 Done!!