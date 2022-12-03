[Day 1] Frameworks Someone's coming to town!

## Best Festival Company Compromised

Someone is trying to stop Christmas this year and stop Santa from delivering gifts to children who were nice this year. The **Best Festival Company’s** website has been defaced, and children worldwide cannot send in their gift requests. There’s much work to be done to investigate the attack and test other systems! The attackers have left a puzzle for the Elves to solve and learn who their adversaries are. McSkidy looked at the puzzle and recognised some of the pieces as the phases of the **Unified Kill Chain**, a security framework used to understand attackers. She has reached out to you to assist them in recovering their website, identifying their attacker, and helping save Christmas.


### Security Frameworks
- Orgs such as Santa's Best Festival Company must adjust and improve their cybersecurity efforts to prevent data breaches.
- Security frameworks come into play to guide in setting up security programs and improve the security posture of the org.

- `Security Frameworks are documented processes that define policies and procedures orgs follow to establish and manage security controls.`
- Frameworks help orgs remove the guesswork of securing their data and infrastructure by establishing processes and structures in a strategic plan.
	- This will also help them achieve commercial and government regulatory requirements.


## Commonly used Frameworks

### 1. NIST Cybersecurity Framework
- The Cybersecurity Framework was developed by the National Institute of Standards and Technology.
- It provides detailed guidance for orgs to manage and reduce cybersecurity risks
- The framework focuses on 5 different functions:
	1. Identify
	2. Project
	3. Detect
	4. Respond 
	5. Recover
- With these functions, the framework allows orgs to prioritise their cybersecurity investments and engage in continuous improvement towards a target cybersecurity profile.

### 2. ISO 27000 Series
- The ISO (International Organization for Standards) develops a series of frameworks for different sectors and industries.
- The ISO 27001 and 27002 standards are commonly known for cybersecurity and outline the requirements and procedures for creating, implementing and managing an InfoSec Management System (ISMS) 
- These standards can be used to access an institution's ability to meet set information security requirements through the application of risk management.

### 3. MITRE ATT&CK Framework
- Identifying adversary plans of attack can be challenging to embark on, blindly.
- They can be understood through the behaviors, methods, tools and strategies established for an attack, commonly known as `Tactics, Techniques and Procedures (TTPs)` 
- The MITRE ATT&CK framework is the knowledge base of TTPs, carefully created and detailed to ensure security teams can identify attack patterns.
	- The framework's structure is similar to that of a periodic table, mapping techniques against phases of the attack chain, and referencing system platforms exploited.


### 4. Cyber Kill Chain
- A key concept of this framework was adopted from the military with the terminology `kill chain` which describes the structure of an attack and consists of target identification, decision and order to attack the target, and finally target destruction.
- Developed by Lockheed Martin, the cyber kill chain describes the stages commonly followed by cyber attacks and security defenders can use the framework as part of intelligence driven defense.

Seven Stages of Cyber KIll Chain framework
1. Recon
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command and Control
7. Action on Objectives
![](../img/Pasted%20image%2020221202152826.png)


### 5. Unified Kill Chain
- As established in our scenario, Santa’s team have been left with a clue on who might have attacked them and pointed out to the Unified Kill Chain (UKC). The Elf Blue Team begin their research.


The UKC is the unification of MITRE ATT&CK and Cyber Kill Chain frameworks.
- Published in 2017 and reviewed in 2022
- The UKC provides a model to defend against cyber attacks from adversary perspective.
- The UKC offers security teams a blueprint for analysing and comparing threat intelligence concering the adversarial mode of working.
- The UKC describes the 18 phases of attack based on TTPs.
	- The individual phases can be combined to form overarching goals, such as gaining a foothold in a targeted network to expand access and performing actions on critical assets.

So we must need to understand these phases from an attack perspective.


Three Cycles of UKC
1. In
2. Through
3. Out

The `In` cycle has the following phases which is focused upon gaining access to a system or networked environment.
- Typically attacks are initiated from an external attacker.
- Steps an attacker would follow are:
1. Recon - Get info about the target from publicly available data
2. Weaponisation - Setting up the required infrastructure to host C2 is crucial in executing attacks
3. Delivery - Payloads delivered to the target such as email phishing and supply chain attacks
4. Social Engineering 
5. Exploitation - If an attacker finds a vulnerability already existing in a software or hardware weakness, they may use it to trigger their payloads
6. Persistence - The attacker will leave behind a fallback presence on the network or asset to make sure they have a point of access to their target.
7. Defence Evasion - The attacker must remain anonymous throughout their exploits by disabling and avoiding any security defence mechanisms in place.
8. C2 - A Communication channel between the compromised system and attacker's infrastructure established across the internet.
- This phase may be considered as a loop as the attacker may be forced to change tactics or modify techniques if one fails to provide a foothold on network

The `Through` phases consists of the following phases which will be towards the goal of gaining elevated access and privileges on assets within a network:
1. Pivoting - The current compromised system can become a launchpad for access towards other systems
2. Discovery - Attacker will go on to gain access and different info about different users on the compromised system. 
3. Privilege Escalation - Restricted access prevents attackers from executing their mission. 
4. Execution - with elevated privileges, malicious code may be downloaded and executed to extract sensitive info
5. Credential Access - Part of extracted sensitive info could include login credentials stored in the memory or disk.
6. Lateral Movement - Using extracted credentials, attacker may move around different systems or data storages within the network.

The `Out` Cycle 
The Confidentiality, Integrity and Availability (CIA) of assets or services are compromised during this phase. Money, fame or sabotage will drive attackers to undertake their reasons for executing their attacks, cause as much damage as possible and disappear without being detected.

1. Collection - After finding the jackpot of data and information, the attacker will seek to aggregate all they need. By doing so, the assets’ confidentiality would be compromised entirely, especially when dealing with trade secrets and financial or personally identifiable information (PII) that is to be secured.
2. Exfiltration - The attacker can must get loot out of this network.
3. Impact - When compromising the availability or integrity of an asset or information, the attacker will use all the acquired privileges to manipulate, interrupt and sabotage. Imagine the reputation, financial and social damage an organisation would have to recover from.
4. Objectives - Attackers may have other goals to achieve that may affect the social or technical landscape that their targets operate within. Defining and understanding these objectives tends to help security teams familiarise themselves with adversarial attack tools and conduct risk assessments to defend their assets.

[UKC White Paper](https://www.unifiedkillchain.com/assets/The-Unified-Kill-Chain.pdf)

[Good Rooms on THM](https://tryhackme.com/module/cyber-defence-frameworks)



## Questions
1. Who is the adversary that attacked Santa's network this year?
Answer - `The Bandit Yeti`

2. What's the flag that they left behind?
Answer - `THM{IT'S A Y3T1 CHR1$TMA$}`

Day 1 Completed!
