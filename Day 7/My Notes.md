[Day 7] CyberChef Maldocs roasting on an open fire

In the previous task, we learned that McSkidy was indeed a victim of a spearphishing campaign that also contained a suspicious-looking document `Division_of_labour-Load_share_plan.doc`. McSkidy accidentally opened the document, and it's still unknown what this document did in the background. McSkidy has called on the in-house expert **Forensic McBlue** to examine the malicious document and find the domains it redirects to. Malicious documents may contain a suspicious command to get executed when opened, an embedded malware as a dropper (malware installer component), or may have some C2 domains to connect to.

Learning Objectives

-   What is CyberChef
-   What are the capabilities of CyberChef
-   How to leverage CyberChef to analyze a malicious document
-   How to deobfuscate, filter and parse the data


### What is CyberChef
- A web-based application, used to slice, encode, decode, parse and analyse data or files.

![](../img/Pasted%20image%2020221207193620.png)

We can also use CyberChef for malicious docs analysis
In previous Day's task we found out there is a malicious doc file called `Division_of_labour-Load_share_plan.doc`

## Let's start
![](../img/Pasted%20image%2020221208233711.png)
- We first provide the file as an input to Cyberchef
- We can use Cyberchef for this potentially malicious doc file analysis
![](../img/Pasted%20image%2020221208233804.png)
- We now get a bunch of gibberish here from the doc.


### Recipe No. 1 (Step 1) - Try to extract strings from the Output
![](../img/Pasted%20image%2020221208233905.png)
- If we examine the result, we can see some random strings of different lengths and some obfuscated strings. Narrow down the search to show the strings with a larger length. Keep increasing the minimum length until you remove all the noise and are only left with the meaningful string
![](../img/Pasted%20image%2020221208234014.png)
- Let's increase the lenght to 257
![](../img/Pasted%20image%2020221208234041.png)
- We see that something meaningful is coming through which could be potentially useful
![](../img/Pasted%20image%2020221208234129.png)

### Recipe No. 2 (Step 2) -  Use Regex to find and replace these strings `[_]` with blank characters so as to eliminate the repeated patterns and hopefully get something as results
![](../img/Pasted%20image%2020221208234334.png)
- We use character escaping here to filter out these repeated patterns.
- We normally filter out characters/patterns in regex using `[]`.
	- We place the pattern we want to find inside these square brackets for them to be considered as a regex
	- Since `[` and `]` are repeating and same as that of regex pattern `[]`, we use character escaping so that both `[` and `]` can be considered a part of regex to be found out and replaced.
	- We also find all the new characters inserted and also the pattern `_` which is a part of `[_]` string.
		- These should be replaced with blank character/nothing.

After all this regex is applied, we get the output above.

- We analyse the output and see that it's a base64 string so we try to then drop the bytes of output which is not the base64!


### Reciple No. 3 (Step 3) - Drop Bytes which are not a part of base64 
- This is mostly Trial and error method.
- We keep increasing the number of bytes to be dropped from Step 2's output until we get the raw base64 output
![](../img/Pasted%20image%2020221208235129.png)
- We see that the threshold is 124 bytes which we dropped.

### Reciple No. 4 (Step 4) - Now we convert from Base64 to simple text output
![](../img/Pasted%20image%2020221208235250.png)
- Hence we see the output as following.
- But we see that there are many unnecessary characters here like `.`, `+`, `()`, `'` and `"`

### Reciple No. 5 (Step 5) - Now powershell scripts are basically encoded in UTF-16LE format by default. So we use the `Decode Text` recipe in order to decode.
![](../img/Pasted%20image%2020221208235725.png)
- Hence we get a more normal output.

### Reciple No. 6 (Step 6) - Now we find strings like `.`, `+`, `()`, `'`, `"` etc and replace them with blank characters/nothing to get a more clearer output
![](../img/Pasted%20image%2020221208235931.png)
We need to find using regex so we use the square brackets `[]` to place out pattern inside.
- We can use `()+'"` or use `|` (OR) regular expression notation to eliminate all the unnecessary characters

Above is the output

### Reciple No. 7 (Step 7) - We now find some strange patterns mentioning google.com but it seems obfuscated
![](../img/Pasted%20image%2020221209000240.png)
- We could maybe find and replace these characters by `http` or `https` which could probably give us some idea of some domain or C2 server the malicious exe must be reaching out to download some sort of stagers or download malicious files
![](../img/Pasted%20image%2020221209000512.png)
- We see some URLs uncover here!!! This is good!

### Reciple No. 8 (Step 8) -  We now extract these URLs using the recipe from Cyberchef
![](../img/Pasted%20image%2020221209000827.png)

### Reciple No. 9 (Step 9) -  Now we will use Split Recipe from Cyberchef to split these URLs from each other 

![](../img/Pasted%20image%2020221209000936.png)
- We here use the Split Delimiter as `@` and Join Delimiter (a different character to be replaced) as `\n` (new line/new line character)
- Here we go! We get a list of URLs which are custom domains the Powershell script was connecting to!


### Reciple No. 9 (Step 9) - Now we defang the URL which is a safety precaution so that the malware analysts might not end up clicking on any of these malicious custom domain links 
![](../img/Pasted%20image%2020221209001236.png)

- Great - We have finally extracted the URLs from the malicious document; it looks like the document was indeed malicious and was downloading a malicious program from a suspicious domain.

- Before passing these domains to the SOC team for deep malware analysis, it is recommended to defang them to avoid accidental clicks. Defanging the URLs makes them unclickable.

## Questions
1. What is the version of CyberChef found in the attached VM?
Answer - `9.49.0`

2. How many recipes were used to extract URLs from the malicious doc?
Answer - `10`

3. We found a URL that was downloading a suspicious file; what is the name of that malware?
Answer - `mysterygift.exe`
![](../img/Pasted%20image%2020221209001419.png)

4. What is the last defanged URL of the bandityeti domain found in the last step?
Answer - `hxxps[://]cdn[.]bandityeti[.]THM/files/index/`
![](../img/Pasted%20image%2020221209001449.png)

5. What is the ticket found in one of the domains? (Format: Domain/<GOLDEN_FLAG>)
Answer - `THM_MYSTERY_FLAG`
`As seen above from the GoldenTicket string containing URL!`


Further Reading for Escaping Strings - [Link](https://stackoverflow.com/questions/10646142/what-does-it-mean-to-escape-a-string)

Further exploration of SIEM - [THM Module SIEM](https://tryhackme.com/module/security-information-event-management)

