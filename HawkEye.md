
# Scenario
## Instructions:
Uncompress the lab (pass: cyberdefenders.org)
## Scenario:
An accountant at your organization received an email regarding an invoice with a download link. 
Suspicious network traffic was observed shortly after opening the email. As a SOC analyst, investigate the network trace and analyze exfiltration attempts.

## Q1. How many packets does the capture have?
Look up for statistics for Packet Lengths, that will show us overall number of packets.
<img width="1919" height="868" alt="image" src="https://github.com/user-attachments/assets/20931fac-0445-448e-b77e-6ebb889280ae" />

## Q2. At what time was the first packet captured?
Just look for first packet Frame. 
<img width="1254" height="667" alt="image" src="https://github.com/user-attachments/assets/c0d3c168-ca20-4e3a-9843-e91998c08eda" />

## Q3. What is the duration of the capture?
Look up in statistics packet capture options.
<img width="1353" height="831" alt="image" src="https://github.com/user-attachments/assets/e3ad01de-167b-4b0b-8603-d0950c350eda" />

## Q4. What is the most active computer at the link level?
<img width="1213" height="413" alt="image" src="https://github.com/user-attachments/assets/792230eb-dcaa-4957-82b6-562aa0c763d6" />
We can lookup the amount of time conversation between computers happened is CONVERSATIONS options. 
Here, the most active one is the one with most packets sent.

## Q5. Manufacturer of the NIC of the most active system at the link level?
We have MAC adress of the most active system from previous question.
We can use a tool like macvendorlookup.com to find the manufacturer.
<img width="1497" height="696" alt="image" src="https://github.com/user-attachments/assets/bf98a09c-c756-48ed-88a6-0063211e7ce4" />

## Q6. Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?
El Paso is answer. Just look up in any web search.

## Q7. The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?
To determine the number of computers in a network capture, it’s important to understand private IP addressing, subnet masks, and broadcast behavior.
In this case, network uses a /24 subnet (255.255.255.0), which supports 254 usable host addresses.
<img width="338" height="91" alt="image" src="https://github.com/user-attachments/assets/027ce2f3-afb3-4623-a87f-3df9bfb6330e" />

## Q8. What is the name of the most active computer at the network level?
To find the host name of most active computer. We will need to find to its DHCP packet containing that information, because DHCP packets often include the hostname of the client device in their options field. First, look up for the most active IP Address in Endpoints panel.
Then we will filter by this IP for DHCP packets and look them up.
<img width="782" height="794" alt="image" src="https://github.com/user-attachments/assets/0077994b-5012-4044-81ad-48e73a205057" />

## Q9. What is the IP of the organization's DNS server?
The IP address of the DNS server can be identified as the source IP in DNS response packets filtered by dns. 10.4.10.4
<img width="326" height="21" alt="image" src="https://github.com/user-attachments/assets/8f760e46-3b73-4ee6-959d-2df0fa953a2c" />

## Q10. What domain is the victim asking about in packet 204?
<img width="1042" height="24" alt="image" src="https://github.com/user-attachments/assets/d19a8690-1134-4a9c-93be-4db0c75a90c7" />
<img width="793" height="453" alt="image" src="https://github.com/user-attachments/assets/1ded6d4b-0f98-47dd-8a2a-c400eb074d4a" />

## Q11. What is the IP of the domain in the previous question?
<img width="1330" height="506" alt="image" src="https://github.com/user-attachments/assets/606dba7e-267d-4cf6-90a8-01aaba270aff" />

## Q12. Indicate the country to which the IP in the previous section belongs.
<img width="925" height="286" alt="image" src="https://github.com/user-attachments/assets/aebe192d-8a00-4132-a6e3-35b9754c5e5d" />

## Q13. What operating system does the victim's computer run?
<img width="1265" height="363" alt="image" src="https://github.com/user-attachments/assets/83376cd8-8984-4f10-8862-2ad8a56b06ce" />
The User-Agent field in HTTP traffic often reveals the operating system.

## Q14. What is the name of the malicious file downloaded by the accountant?
Go to File > Export Objects > HTTP in Wireshark to see a list of downloaded files, including the malicious one.
<img width="886" height="174" alt="image" src="https://github.com/user-attachments/assets/b70a88dc-8797-4fec-ab8c-f20a0e31eb82" />

## Q15. What is the md5 hash of the downloaded file?
<img width="514" height="66" alt="image" src="https://github.com/user-attachments/assets/b3f164fe-34d8-4a6a-8e93-ca4fed8d94dc" />

## Q16. What software runs the webserver that hosts the malware?
Check the HTTP response headers for the server information.
<img width="460" height="57" alt="image" src="https://github.com/user-attachments/assets/161f30af-99fa-4f82-95eb-dff0e1eab5ad" />

## Q17. What is the public IP of the victim's computer?
The public IP will appear as the source address in packets leaving the network from the internal IP (10.4.10.132). Focus on packets where the victim's internal IP communicates with external IPs Go to packet 3166 and look at the Line-based text data field.
<img width="1136" height="635" alt="image" src="https://github.com/user-attachments/assets/67377820-309c-43ab-b2e5-3c0b1061a0b3" />

## Q18. In which country is the email server to which the stolen information is sent?
Look for SMTP protocol for emails. Identify the email server's IP from SMTP traffic and use a geolocation service.<img width="819" height="214" alt="image" src="https://github.com/user-attachments/assets/b5ce6bb5-b537-4ec7-b870-887371cb06b1" />

## Q19. Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?
 Received header in the email's SMTP traffic often includes the software running on the email server. Follow TCP stream and analyze the SMTP conversation.
<img width="1876" height="690" alt="image" src="https://github.com/user-attachments/assets/371d4476-b8bf-4957-b5f4-3a988932e31d" />

## Q20. To which email account is the stolen information sent?
The recipient's email address should be visible in the SMTP traffic.
<img width="1272" height="442" alt="image" src="https://github.com/user-attachments/assets/2373e868-5436-492d-8f32-1a53e8784349" />

## Q21. What is the password used by the malware to send the email?
Look for base64 or other encoded strings in SMTP traffic. 
SMTP traffic can expose sensitive credentials when sent over unencrypted channels. In this case, the captured stream contained a Base64-encoded password used by the malware to log in to the attacker’s email server. 
By extracting the encoded string from the authentication packet and decoding it in CyberChef.
<img width="1448" height="590" alt="image" src="https://github.com/user-attachments/assets/eceeec69-76e4-4813-855f-4e840a89570f" />

## Q22. Which malware variant exfiltrated the data?
I analyzed the behavior and communication patterns in SMTP traffic, and found interesting base code strings.
<img width="813" height="811" alt="image" src="https://github.com/user-attachments/assets/a6a03c70-2e11-451c-a44b-a0901808ecfc" />

## Q23. What are the bankofamerica access credentials? (username:password)
<img width="824" height="504" alt="image" src="https://github.com/user-attachments/assets/ce555ed4-9377-453b-8d5d-a14b33ef5108" />

## Q24. Every how many minutes does the collected data get exfiltrated?
Every 10 minutes. Just look at the timestamps in the SMTP traffic for the emails sent by the malware and calculate the interval.
<img width="511" height="18" alt="image" src="https://github.com/user-attachments/assets/d91f03e1-0b57-447f-ae71-48c70dffd9df" />
<img width="528" height="28" alt="image" src="https://github.com/user-attachments/assets/d7c2f556-fa20-48ab-9d80-777c6b807d49" />


This was easy lab, but boring one. We will see the next one later. 


