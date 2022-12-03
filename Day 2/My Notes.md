[Day 2] Log Analysis Santa's Naughty & Nice Log

Santa's SOC has noticed one of their web servers, santagift.shop has been hijacked by the Bandit Yeti APT group.
- Elf McBlue's task is to analyse the log files captured from the web-server to understand what is happening and track down the said APT group.


## Learning Objectives

In today’s task, you will:
-   Learn what log files are and why they’re useful
-   Understand what valuable information log files can contain
-   Understand some common locations these logs file can be found
-   Use some basic Linux commands to start analysing log files for valuable information
-   Help Elf McBlue track down the Bandit Yeti APT!


## What are log files?
- Log files are files that contain historical records of events and other data from the application.

Some common examples of events found in a log file are:
1. Login attempts or failures
2. Traffic on a network
3. Website, URLs etc that have been accessed on your application.
4. Password changes
5. Application errors (which are useful for debugging)

Log files help us investigate the following:
1. What has happened?
2. When has it happened?
3. Where has it happened?
4. Who do it? Why are they successful?
5. What is the result of this action?

- For example, a system administrator may want to log the traffic happening on a network. We can use the logging to answer questions in case of an event/incident

_A user has reportedly accessed inappropriate material on a University network._ With logging in place, a systems administrator could determine the following:
![](../img/Pasted%20image%2020221203145906.png)

## What does a log file look like?
Some of the following attributes a log file could contain are:
1. Timestamp of the event
2. The name of the service that is generating the log file (like SSH)
3. The actual event the service logs (event of failed authentication)

![](../img/Pasted%20image%2020221203150128.png)

### Common Locations of Log files?
- Windows features has a built in application that allows us to access log files 

The `Event Viewer` is the utility we are looking for which stores all the types of logs in Windows system
![](../img/Pasted%20image%2020221203150907.png)

Types of Event Viewer logs are:-
1. Application - these are all the logs related to application on the system. Example is to see when a service was started or stopped
2. Security - All the logs which display the logons and Special Logons to the system related to Security
3. Setup - This contains all the events related to system's maintenance. Windows update logs are stored here
4. System - all logs related to the system itself. Example of system events are if a USB has been plugged in or removed or system shutdown info

We above saw types of logs in Windows, now are going to look at logs in Linux!!
- Logs are stored in `/var/log` folder of Linux.
- OS logs (and often software specific logs) are located in this directory
![](../img/Pasted%20image%2020221203151418.png)

The table above highlights the following log files:
1. Authentication - This file contains all authentication. This is usually attempted either remotely or on the system itself.
File (Ubuntu) - auth.log
- Example - Failed password for root from x.x.x.x IP port 22 ssh2

2. Package Management - This log file contains all events related to package management on system. When installing a new software(package), this is logged in this file. This is useful for debugging and reverting changes in case the installation causes unintended behavior on the system.
File (Ubuntu) - dpkg.log
- Example - 2022-06-03 21:45:59 installed neofetch

3. Syslog - This log file contains all events related to things happening in the system's background. For example, crontabs executing, services starting and stopping, or other automatic behaviours such as log rotation. This file can help debug problems.
File (ubuntu) -  syslog
- Example - 2022-06-03 13:33:7 Finished Daily apt download activities.

4. Kernel - this log file contains all events related to the kernel itself. For example, changes to the kernel, or output from devices such as networking equipment or physical devices such as USB.
File (Ubuntu) - kern.log
- Example - 2022-06-03 10:10:01 Firewalling registered


Log files can quickly contain many events and hundreds, if not thousands, of entries. The difficulty in analysing log files is separating useful information from useless. Tools such as Splunk are software solutions known as Security Information and Event Management (SIEM) is dedicated to aggregating logs for analysis. Listed in the table below are some of the advantages and disadvantages of these platforms:
![](../img/Pasted%20image%2020221203153106.png)
- But here we will use "Grep" to explore through all the things as we don't have a SIEM yet


## DIY - Read man pages for grep!


## Questions - Using Walkthrough
1. Use the `ls` command to list the files present in the current directory. How many log files are present?
![](../img/Pasted%20image%2020221203153442.png)
Answer - `2`

2. Elf McSkidy managed to capture the logs generated by the web server. What is the name of this log file?
Answer - `webserver.log`

3. Begin investigating the log file from question #3 to answer the following questions.
`No answer needed`

4. On what day was Santa's naughty and nice list stolen?
Answer - `friday`
![](../img/Pasted%20image%2020221203153625.png)
- The attacker was fuzzing the web server directories on 18th Nov 2022, so that narrows it down to Friday.

5. What is the IP address of the attacker?
Answer - `10.10.249.191`

6. What is the name of the important list that the attacker stole from Santa?
Answer - `santaslist.txt`
![](../img/Pasted%20image%2020221203153820.png)

7. Look through the log files for the flag. The format of the flag is: THM{}
![](../img/Pasted%20image%2020221203153903.png)
Answer - `THM{STOLENSANTASLIST}`
- We used regular expression `THM*` to search for the string starting with THM followed by any character!

Day 2 Completed!

[To explore more about Enterprise Solutions, do the following module](https://tryhackme.com/module/endpoint-security-monitoring)
