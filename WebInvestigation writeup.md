<img width="1342" height="282" alt="image" src="https://github.com/user-attachments/assets/b04d8e0f-4ff4-4260-947b-f6abcaf58884" />

# Scenario
You are a cybersecurity analyst working in the Security Operations Center (SOC) of BookWorld, an expansive online bookstore renowned for its vast selection of literature. BookWorld prides itself on providing a seamless and secure shopping experience for book enthusiasts around the globe. Recently, you've been tasked with reinforcing the company's cybersecurity posture, monitoring network traffic, and ensuring that the digital environment remains safe from threats.
Late one evening, an automated alert is triggered by an unusual spike in database queries and server resource usage, indicating potential malicious activity. This anomaly raises concerns about the integrity of BookWorld's customer data and internal systems, prompting an immediate and thorough investigation.
As the lead analyst in this case, you are required to analyze the network traffic to uncover the nature of the suspicious activity. Your objectives include identifying the attack vector, assessing the scope of any potential data breach, and determining if the attacker gained further access to BookWorld's internal systems.

## Q1. By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?  
We will use the Statistics tab in Wireshark, then navigate to >Conversations to see which IP addresses are communicating most actively and suspicious activities.
<img width="1567" height="57" alt="image" src="https://github.com/user-attachments/assets/abfe626d-1a89-4457-a908-c03dcdff70bc" />
We see the unusual big traffic compared to other Conversations on this IP.
Answer: 111.224.250.131
## Q2. If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?  
<img width="763" height="271" alt="image" src="https://github.com/user-attachments/assets/84f140f3-0ce6-4056-86f8-27dd5aa4bdfe" />
Use any open IP address location capturing site.
Answer: Shijiazhuang
## Q3. Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable PHP script name?  
We will observe all HTTP requests and search for any nonstandart script names.
<img width="1868" height="468" alt="image" src="https://github.com/user-attachments/assets/a5bec244-061c-47ce-ba82-f97752c28a0c" />
There it is, very clear indication of script that attacker used.
Answer: search.php
## Q4. Establishing the timeline of an attack, starting from the initial exploitation attempt, what is the complete request URI of the first SQLi attempt by the attacker?  
Considering that attacker injected script, we will look for any recon techniques of SQL injection attack. In this case, it should be any SQL query that is checking for website capabilities of SQL query sanitization.
<img width="1377" height="43" alt="image" src="https://github.com/user-attachments/assets/94c66abd-a27d-43de-9aab-5c403cc556a6" />
We found it.
Answer: /search.php?search=book and 1=1; -- -
## Q5. Can you provide the complete request URI that was used to read the web server's available databases?  
Lets examine http requests that contains full SQL query requests, SQL request should contain the request that retrieves information needed for attacker.
Many requests include the UNION SELECT SQL statement, a method commonly used to retrieve data from additional tables within the database.
In this case, we should look for query strings that exploit the INFORMATION_SCHEMA database, which is a system database in SQL environments storing metadata about other databases, tables, and columns
<img width="1894" height="64" alt="image" src="https://github.com/user-attachments/assets/a842409f-fdb5-4937-b113-e2edb59e096b" />
<img width="219" height="284" alt="image" src="https://github.com/user-attachments/assets/494a6f35-60b0-434f-841f-dfcb227666db" />
Answer: /search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,schema_name)),0x7176706a71) FROM INFORMATION_SCHEMA.SCHEMATA-- -
## Q6. Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?  
Thank to previous discovery, we know what db is used in this attack. So we will filter to find any requests containing this db. 
<img width="1295" height="343" alt="image" src="https://github.com/user-attachments/assets/91d1686a-dd4b-4390-98c1-ff1d3de77dea" />
We found 3 results.<img width="232" height="58" alt="image" src="https://github.com/user-attachments/assets/77c3cd5c-a8d9-4745-9eed-a0621d599736" />
1st one is admin. But its not the table that contains users data. So next.
<img width="1223" height="630" alt="image" src="https://github.com/user-attachments/assets/ea48e5e1-8479-4fac-869f-77ffa75d3d8b" />
2nd one is correct. It has everything we searched for.
Answer: customers
## Q7. The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide the name of the directory discovered by the attacker?  
Since attacker escalated his privileges, we should search for any admin directories. In this case, we can narrow down the search by filtering for "POST" API requests, that is used for logging in and posting any scripts.
<img width="1605" height="179" alt="image" src="https://github.com/user-attachments/assets/900d4679-abfc-4de3-a396-b85bef385758" />
Answer: /admin/
## Q8. Knowing which credentials were used allows us to determine the extent of account compromise. What are the credentials used by the attacker for logging in?  
<img width="301" height="184" alt="image" src="https://github.com/user-attachments/assets/e4ec37a0-7859-41cf-bb1b-22d3a98b9f84" />
Answer: admin:admin123!
## Q9. We need to determine if the attacker gained further access or control of our web server. What's the name of the malicious script uploaded by the attacker?
<img width="678" height="51" alt="image" src="https://github.com/user-attachments/assets/cf586d3e-00ad-4314-9f96-8d348666f15e" />
Answer: NVri2vhp.php
<img width="603" height="111" alt="image" src="https://github.com/user-attachments/assets/22541b89-b731-4135-8b08-ec1d62c8cb7a" />
