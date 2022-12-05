[Day 3] OSINT Nothing escapes detective McRed

As the elves are trying to recover the compromised `santagift.shop` website, elf Recon McRed is trying to figure out how it was compromised in the first place. Can you help him in gathering open-source information against the website?

**Learning Objectives**

-   What is OSINT, and what techniques can extract useful information against a website or target?
-   Using dorks to find specific information on the Google search engine
-   Extracting hidden directories through the Robots.txt file
-   Domain owner information through WHOIS lookup
-   Searching data from hacked databases
-   Acquiring sensitive information from publicly available GitHub repositories

### What is OSINT?
- It is gathering publicly available data for intelligence purposes, which includes info collected from internet, mass media, specialist journals, photos and geospatial information.
- The information can be accessed via open internet(search engines), closed forums and even the deep dark web.
	- People tend to leave much info on internet that is publicly available and later results in identity theft, impersonation etc.

## OSINT Techniques
#### 1. Google Dorking
- This action involves using specialist terms and advanced search operators to find results that are not usually displayed in regular search terms.
	- You can use them to find specific filetypes, cached version of a particular website, websites containing text etc.
	- Adversaries always use this technique to find web config files and loopholes left due to bad coding practices.
- Some of the widely used Google Dorks are 
1. `inurl` - searches for a specific text in all indexed URLs. 
- Example - `inurl:hacking` searches/indexes all the URLs containing the word hacking
2. `filetype` - searches for specific file extensions.
- Example - `filetype:pdf "hacking"` will bring pdf files containing the word "hacking"
3. `site` - searches all the indexed URLs for the specific domain. 
- Example - `site:"tryhackme.com"` will index all URLs from domain "tryhackme.com"
4. `cache` - Get latest cached version by the Google search engine
- Example - `cache:tryhackme.com` 

For example, we could also use the dork `site:github.com "DB_PASSWORD"` to search only in `Github.com` and look for the string `DB_PASSWORD` 

#### 2. WHOIS Lookup
- Whois database stores public domain info such as registrant (domain owner), administrative, billing and technical contacts in a centralised database.
- The database is publicly available to search against any domain and enables acquiring PII against a company like email addresses, phone numbers etc
- These emails could be used by adversaries for `spear phishing campaigns` 
- Nowadays registrars offer Domain Privacy Options that allow users to keep whois information private from the general public and only accessible to certain registrars only.

#### 3. robots.txt on the website
- The robots.txt is a publicly accessible file created by the website administrator and intended for search engines to allow or disallow indexing of the website's URLs. All websites have their robots.txt file directly accessible through the domain's main URL. It is a kind of communication mechanism between websites and search engine crawlers. Since the file is publicly accessible, it doesn't mean anyone can edit or modify it. You can access robots.txt by simply appending robots.txt at the end of the website URL.

#### 4. Breached DB search
- Major social media and tech giants have suffered data breaches in the past.  
- As a result, the leaked data is publicly available and, most of the time contains PII like usernames, email addresses, mobile numbers and even passwords. 
	- Users may use the same password across all the websites; that enables bad actors to re-use the same password against a user on a different platform for a complete account takeover. Many web services offer to check if your email address or phone number is in a leaked database; [HaveIBeenPwned](https://haveibeenpwned.com/) is one of the free services.

#### 5. Searching GitHub Repos
- GitHub is a renowned platform that allows developers to host their code through version control. A developer can create multiple repositories and set the privacy setting as well. 
- A common flaw by developers is that the privacy of the repository is set as public, which means anyone can access it. These repositories contain complete source code and, most of the time, include passwords, access tokens, etc.


#####  Objectives
- McRed, the recon master, searched various terms on GitHub to find something useful like `SantaGiftShop`, `SantaGift`, `SantaShop` etc. Luckily, one of the terms worked, and he found the website's complete source code publicly available through OSINT.


## Questions/ Walkthrough

1. What is the name of the Registrar for the domain santagift.shop?
Answer - `Namecheap, Inc.`
![](../img/Pasted%20image%2020221205062858.png)

2. Find the website's source code (repository) on [github.com](https://github.com/) and open the file containing sensitive credentials. Can you find the flag?
Answer - `{THM_OSINT_WORKS}`
![](../img/Pasted%20image%2020221205063011.png)
- We can go to github and search for the keywords like `Santagift`, `SantaGiftShop`

3. What is the name of the file containing passwords?
Answer - `config.php`

- Note - Check the main page of github repo and there's something related to it ;)

4. What is the name of the QA server associated with the website?
Answer - `qa.santagift.shop`
![](../img/Pasted%20image%2020221205070304.png)

5. What is the DB_PASSWORD that is being reused between the QA and PROD environments?
Answer - `S@nta2022`
![](../img/Pasted%20image%2020221205070344.png)

Day 3 Done!!

