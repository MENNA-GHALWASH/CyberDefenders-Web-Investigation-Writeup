# Web Investigation Lab - Write up

Website - 

[https://cyberdefenders.org/blueteam-ctf-challenges/web-investigation/](https://cyberdefenders.org/blueteam-ctf-challenges/web-investigation/)

Category: [**Network Forensics**](https://cyberdefenders.org/blueteam-ctf-challenges/?categories=network-forensics)

Tactics: [**Initial Access](https://cyberdefenders.org/blueteam-ctf-challenges/?tactics=initial-access)[Persistence](https://cyberdefenders.org/blueteam-ctf-challenges/?tactics=persistence)[Command and Control](https://cyberdefenders.org/blueteam-ctf-challenges/?tactics=command-and-control)**

Tools: [**Wireshark](https://cyberdefenders.org/blueteam-ctf-challenges/?tools=wireshark)[Network Miner](https://cyberdefenders.org/blueteam-ctf-challenges/?tools=network-miner)**

### scenario:

You are a cybersecurity analyst working in the Security Operations Center (SOC) of BookWorld, an expansive online bookstore renowned for its vast selection of literature. BookWorld prides itself on providing a seamless and secure shopping experience for book enthusiasts around the globe. Recently, you've been tasked with reinforcing the company's cybersecurity posture, monitoring network traffic, and ensuring that the digital environment remains safe from threats.

Late one evening, an automated alert is triggered by an unusual spike in database queries and server resource usage, indicating potential malicious activity. This anomaly raises concerns about the integrity of BookWorld's customer data and internal systems, prompting an immediate and thorough investigation.

As the lead analyst in this case, you are required to analyze the network traffic to uncover the nature of the suspicious activity. Your objectives include identifying the attack vector, assessing the scope of any potential data breach, and determining if the attacker gained further access to BookWorld's internal systems.

---

## **Questions:**

Q1

**By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?**

we went through wireshark and looked for a specific ip address with many requests - and found an interesting ip that shows requests thatlook suspicouosly like an attempt at sql injection

the logic is that - attackers will use automated tools to attempt to find the correct vulnerability and exploit, and use nmap for multiple ping requests that are abnormally numerous

the ip found was: 

ans - **111.224.250.131**

---

Q2

**If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?**

that one was easy - simply go to a website that changes the ip address to a geographical location such as [geolocation](https://www.geolocation.com/?ip=111.224.250.131#ipresult) - which will give you the following results:

![Screenshot from 2026-08-20 18-54-37.png](Web%20Investigation%20Lab%20-%20Write%20up/Screenshot_from_2026-08-20_18-54-37.png)

ans - Shijiazhuang

---

Q3

**Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable PHP script name?**

the steps were as follows, we filtered the http requests, searched for the successful request, expanded the network packet and headed down till the exploit file was found, which lead us to the file:

ans - search.php

Q4

**Establishing the timeline of an attack, starting from the initial exploitation attempt, what is the complete request URI of the first SQLi attempt by the attacker?**

Note: Decode the Value.

the logic went as follows - we filtered for a select string as its the usual sql injection tactic deployed, found that the first request was encoded, went to cyberchef and decoded the string and the final request turned out to be:

ans - **/search.php?search=book and 1=1; -- -**

Q5

**Can you provide the complete request URI that was used to read the web server's available databases?**

Note: Decode the Value.

this is where the real work begins on the attacker’s side, they’d search for the information schema of the database through injecting the sql command within the url.

a simple filter of the requests with the keyword information schema lead us to the request:

ans - **/search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,schema_name)),0x7176706a71) FROM INFORMATION_SCHEMA.SCHEMATA-- -**

Q6

**Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?**

this one was slightly tricky, we had to know the tables existing to narrow them down, so logically the attacker would attempt another sql statement to view all tables, so he would use the table-name sql command injected in the url. our work was to search for that request through filtering it and then finding its response as an http response so it’s human readable. we found multiple table names- admin, and others, and among those was the table:

ans - **customers**

Q7

**The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide the name of the directory discovered by the attacker?**

this was about directory enumeration, where the attacker would attempt a couple of web requests with a wordlist containing multiple potential webpage name, so we had to filter for http requests with the url and unknown subdomains with a response of 200, we found:

ans **- /admin/**

Q8

**Knowing which credentials were used allows us to determine the extent of account compromise. What are the credentials used by the attacker for logging in?**

this is where we’ll have to filter on the hidden website admin and view the request details. we will find multiple attempts at a username and a password, the final one is the correctr one - naturally, as they will have succeeded in their pursuit.

ans - **admin:admin123!**

Q9

**We need to determine if the attacker gained further access or control of our web server. What's the name of the malicious script uploaded by the attacker?**

this is a classic case of file upload, so filter for http request with .php on wireshark, and by looking at the request details you will find:

ans - **NVri2vhp.php**

by MennaKmg