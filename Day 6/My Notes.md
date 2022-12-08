[Day 6] Email Analysis It's beginning to look a lot like phishing

Elf McBlue found an email activity while analysing the log files.
- It looks like everything started with an email.

## Learning Objectives
1. Learn what the email analysis is and why it matters
2. Learn the email header sections
3. Learn essential questions to ask in email analysis 
4. Learn how to use additional tools to discover email attachments and conduct further analysis
5. Help Elf team investigate suspicious email received.


## What is Email Analysis?
- Process of extracting the email header information to expose email file details.
- The email header contains technical details of email like sender, recipient, path, return address and attachments.
- Usually these details are enough to determine if something suspicious/abnormal is present in email and decide on further interactions on email, like filtering/quarantining or delivering.
	- This process can be manually done with the help of tools.


There are 2 concerns in email analysis
1. `Security Issues`
- Identifying suspicious/malicious patterns in email 

2. `Performance Issues`
- Identifying delivery and delay issues in emails.

In this task, we will focus on security concerns on emails, a.k.a. phishing. Before focusing on the hands-on email analysis, you will need to be familiar with the terms "social engineering" and "phishing".

1. `Social Engineering`
- Psychological manipulation of people into performing or divulging info by exploiting weakness in human nature.
- These weaknesses can be curiosity, jealousy, greed, kindness and willingness to help someone.

2. `Phishing`
- Subsection of social engineering delivered through email to trick the recipient into clicking a malicious link, which leads to running a malicious script on a computer, revealing personal info and credentials

[Look more here for phishing](https://tryhackme.com/module/phishing)


## Does email analysis still matter?
- Yes! Various academic research and technical reports highlight that phishing attacks are still extremely common, effective and difficult to detect. It is also part of penetration testing and red teaming implementations (paid security assessments that examine organisational cybersecurity). Therefore, email analysis competency is still an important skill to have. Today, various tools and technologies ease and speed up email analysis. Still, a skilled analyst should know how to conduct a manual analysis when there is no budget for automated solutions. It is also a good skill for individuals and non-security/IT people!

Note - In-depth analysis requires an isolated requirement to work.
- It is only suggested to download and upload received emails and attachments if you are in an authorised team and have an isolated environment.
- Suppose you are outside the corresponding team or a regular user. In that case, you can evaluate the email header using the raw format and conduct the essential checks like the sender, recipient, spam score and server information. Remember that you have to inform the corresponding team afterwards.


### Before diving into email analysis
- We need to know the structure of an email header.

![](../img/Pasted%20image%2020221207180936.png)


### Important Email Header Fields for Quick Analysis
- Analysing multiple header fields can be confusing at first, but starting from the key points will make the process slightly easier.
- A simple process of email analysis is show below

![](../img/Pasted%20image%2020221207181134.png)
- You will also need a email header parser tool or configure a text editor to highlight and spot the email address' headers easily
- The difference between raw and parsed views of email headers is show below
![](../img/Pasted%20image%2020221207181351.png)

You can use Sublime Text to view email files without opening and executing any of the linked attachments/commands. You can view the email file in the text editor using two approaches.

-   Right-click on the sample and open it with Sublime Text.
-   Open Sublime Text and drag & drop the sample into the text editor.


- If your file has a `".eml"` or `".msg"` extension, Sublime Text will automatically detect the structure and highlight the header fields for ease of readability
	- Note that if your using a `".txt"` you will need to manually select the highlighting format using the button located at lower right corner.


## Tools for Email Analysis - emlAnalyzer
Text editors are helpful in analysis, but there are some tools that can help you to view the email details in a clearer format. In this task, we will use the "emlAnalyzer" tool to view the body of the email and analyse the attachments. The emlAnalyzer is a tool designed to parse email headers for a better view and analysis process. The tool is ready to use in the given VM. The tool can show the headers, body, embedded URLs, plaintext and HTML data, and attachments. The sample usage query is explained below.

##### Syntax of emlAnalyzer
![](../img/Pasted%20image%2020221207181757.png)

```
user@ubuntu$ emlAnalyzer -i Urgent\:.eml --header --html -u --text --extract-all
```

At this point, you should have completed the following checks.  

-   Sender and recipient controls
-   Return path control
-   Email server control
-   Message-ID control
-   Spam value control 
-   Attachment control (Does the email contains any attachment?)


Additionally, we can also use some OSINT tools to check email reputation and enrich the findings.
- Visit the site below to check on sender and email addresses mentioned in the return path header.
`https://emailrep.io site is useful to check the emails seen during analysis`

- Here, if you find any suspicious URLs and IP addresses, consider using some OSINT tools for further investigation. While we will focus on using Virustotal and InQuest, having similar and alternative services in the analyst toolbox is worthwhile and advantageous.
![](../img/Pasted%20image%2020221207182316.png)


- After completing these initial checks, you can continue with body and attachment analysis. 

## Questions
1. What is the email address of the sender?
Answer - `chief.elf@santaclaus.thm`
![](../img/Pasted%20image%2020221208093855.png)

2. What is the return address?
Answer - `murphy.evident@bandityeti.thm`
![](../img/Pasted%20image%2020221208094001.png)

3. On whose behalf was the email sent?
Answer - `Chief Elf`

4. What is the X-spam score?
Answer - `3`
![](../img/Pasted%20image%2020221208094131.png)

5. What is hidden in the value of the Message-ID field?
Answer - `AoC2022_Email_Analysis`
![](../img/Pasted%20image%2020221208094252.png)

6. Visit the email reputation check website provided in the task.  
What is the reputation result of the sender's email address?
Answer - `RISKY`
![](../img/Pasted%20image%2020221208094405.png)

7. Check the attachments.  
What is the filename of the attachment?
Answer - `Division_of_labour-Load_share_plan.doc`
![](../img/Pasted%20image%2020221208094844.png)

8. What is the hash value of the attachment?
Answer - `0827bb9a2e7c0628b82256759f0f888ca1abd6a2d903acdb8e44aca6a1a03467`
![](../img/Pasted%20image%2020221208095008.png)

9. Visit the Virus Total website and use the hash value to search.  
Navigate to the behaviour section.  
What is the second tactic marked in the Mitre ATT&CK section?
Answer - `Defence Evasion`
![](../img/Pasted%20image%2020221208095348.png)

10. Visit the InQuest website and use the hash value to search.  
What is the subcategory of the file?
Answer - `macro_hunter`


## Steps of Email Analysis covered in this rool
1. Open the `".eml"` file through Sublime text
2. Observe all the email headers carefully
3. Use `emlAnalyzer` to extract all the attachments and imp info like links from the email file
4. Then use `https://emailrep.io` to check the sender and return path email addresses of the email
5. If any attachments in the mail, calculate the SHA-256 hash and look it up on Virustotal and [Labs Inquest](https://labs.inquest.net/)
6. 


































