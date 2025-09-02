# BlueSky Ransomware Attack writeup cyberdefenders
## Scenario
A high-profile corporation, responsible for managing **critical data and services across diverse industries**, has reported a **major security incident**.
Their network has been impacted by a **suspected ransomware attack**. Key files have been encrypted, causing service disruptions and raising concerns about **potential data compromise**.

Preliminary indicators suggest the involvement of a **sophisticated threat actor**.

# Walkthrough of the incident
First, we need to determine how attacker gained access to network, do reconnaissance work. Lets answer questions. 

## Q1 - Knowing the source IP of the attack allows security teams to respond to potential threats quickly. 
Can you identify the source IP responsible for potential port scanning activity?: 
We can detect all acitivites simply by accessing Statistics, where we can look for all suspicious activites. In this case i found **87.96.21.84** pinging several ports.
<img width="1469" height="611" alt="image" src="https://github.com/user-attachments/assets/7e0aebb7-025c-4952-8395-dd35b63f999d" />

## Q2 - During the investigation, it's essential to determine the account targeted by the attacker. Can you identify the targeted account username?
TDS traffic is unusually, Tabular Data Stream (TDS) is an application layer protocol used to transfer data between a database server and a client.
It works with MC SQL servers, so naturally we begin to investigate TDS traffic info. By checking for login attempts and related metadata.
<img width="911" height="628" alt="image" src="https://github.com/user-attachments/assets/b5a75363-40b5-47d1-a0ca-e6f4b8e3ea02" />

## Q3 - We need to determine if the attacker succeeded in gaining access. Can you provide the correct password discovered by the attacker?
Again we check TDS login packet
<img width="431" height="39" alt="image" src="https://github.com/user-attachments/assets/0dcd73ef-4844-40fc-8979-74d29d4a4a34" />

## Q4 - Attackers often change some settings to facilitate lateral movement within a network. What setting did the attacker enable to control the target host further and execute further commands?
When TDS transfers data, it also transfers SQL statements together from the client to server. I did look for SQL batches that contained SQL statements.<img width="1254" height="685" alt="image" src="https://github.com/user-attachments/assets/a5a5e2ac-6c3c-4f1d-8a62-bb4ce2ddc8f0" />
it configures that attacker can use SQL queries as system commands, which is dangerous.

## Q5 - Process injection is often used by attackers to escalate privileges within a system. What process did the attacker inject the C2 into to gain administrative privileges?
Look into event viewer and filter for powershell events.
<img width="962" height="708" alt="image" src="https://github.com/user-attachments/assets/3909d739-4d63-440d-87b4-5a8c3a35c98c" />

## Q6 -Following privilege escalation, the attacker attempted to download a file. Can you identify the URL of this file downloaded?
I filtered for HTTP protocols and quickly found exact HTTP requests. It is http://87.96.21.84/checking.ps1
<img width="1039" height="555" alt="image" src="https://github.com/user-attachments/assets/44f4d006-0757-4212-bbfd-cf09485d9505" />
<img width="392" height="59" alt="image" src="https://github.com/user-attachments/assets/da619bd9-8cfd-4934-8f19-5836da9ae401" />

## Q7 - Understanding which group Security Identifier (SID) the malicious script checks to verify the current user's privileges can provide insights into the attacker's intentions. Can you provide the specific Group SID that is being checked?
We will check packet for this command.<img width="1018" height="607" alt="image" src="https://github.com/user-attachments/assets/8faab652-6b28-4653-9274-6ce0d8b7d260" />

## Q8 - Windows Defender plays a critical role in defending against cyber threats. If an attacker disables it, the system becomes more vulnerable to further attacks. What are the registry keys used by the attacker to disable Windows Defender functionalities? Provide them in the same order found.
<img width="788" height="239" alt="image" src="https://github.com/user-attachments/assets/93c3670b-5ed5-4993-b6a4-0f1e2d2b2ed4" />

## Q9 - Can you determine the URL of the second file downloaded by the attacker?
<img width="1152" height="47" alt="image" src="https://github.com/user-attachments/assets/bb46b91b-41c2-4dd8-b267-20585d4f112b" />

## Q10 - Identifying malicious tasks and understanding how they were used for persistence helps in fortifying defenses against future attacks. What's the full name of the task created by the attacker to maintain persistence?
Look for commands using schtasks.exe in the PowerShell script. Examine the script for task creation commands with SYSTEM privilege<img width="1246" height="559" alt="image" src="https://github.com/user-attachments/assets/b9d23a32-bf21-4b2d-8bc3-537f71605b23" />

## Q11 - Based on your analysis of the second malicious file, What is the MITRE ID of the main tactic the second file tries to accomplish?
I searched the code on the internet, so by searching it seems like Defence Evasion, because script tries to cover up their leads.

## Q12 - What's the invoked PowerShell script used by the attacker for dumping credentials? 
<img width="1051" height="58" alt="image" src="https://github.com/user-attachments/assets/bc0ca547-d5e5-46f4-89e3-c8e7e02804c9" />

## Q13 - Understanding which credentials have been compromised is essential for assessing the extent of the data breach. What's the name of the saved text file containing the dumped credentials?
We are looking through the attackers files, scripts, or captured traffic for any text file that contains IP addresses or hostnames
<img width="1027" height="841" alt="image" src="https://github.com/user-attachments/assets/c6dbaf3b-2ee1-4d9d-acf6-b78dfe7bb019" />

## Q14 - Knowing the hosts targeted during the attacker's reconnaissance phase, the security team can prioritize their remediation efforts on these specific hosts. What's the name of the text file containing the discovered hosts?
<img width="1116" height="42" alt="image" src="https://github.com/user-attachments/assets/6943c7c4-8001-4e3e-a45c-dcb013338fc4" />

## Q15 - After hash dumping, the attacker attempted to deploy ransomware on the compromised host, spreading it to the rest of the network through previous lateral movement activities using SMB. You’re provided with the ransomware sample for further analysis. By performing behavioral analysis, what’s the name of the ransom note file?
We wil export jawaw.exe, cause its the last file uploaded post execution. We export the file and get the hash and go to VirusTotal to check. <img width="1463" height="780" alt="image" src="https://github.com/user-attachments/assets/93d14044-296a-46d1-85b5-f2436c529b35" />
<img width="946" height="490" alt="image" src="https://github.com/user-attachments/assets/0d6e7511-bcfe-4720-ae9e-a2b5d61ae75f" />

## Q16 - In some cases, decryption tools are available for specific ransomware families. Identifying the family name can lead to a potential decryption solution. What's the name of this ransomware family?
BlueSky



