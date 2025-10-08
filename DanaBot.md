<img width="1319" height="288" alt="image" src="https://github.com/user-attachments/assets/169d3dca-38d0-4d30-83be-d41355a2ca40" />

# Scenario
The SOC team has detected suspicious activity in the network traffic, revealing that a machine has been compromised. Sensitive company information has been stolen. 
Your task is to use Network Capture (PCAP) files and Threat Intelligence to investigate the incident and determine how the breach occurred.

## Q1. Which IP address was used by the attacker during the initial access?
<img width="1823" height="190" alt="image" src="https://github.com/user-attachments/assets/176a7b72-196a-4ec9-8fbf-3f86b5ba08f7" />
We see that DNS query returns *62.173.142.148* for domain, lets check domain in VirusTotal.
<img width="1249" height="403" alt="image" src="https://github.com/user-attachments/assets/a385ecfa-e9f1-426c-8ad6-847c64cfc489" />
As we see this is attack angle.
## Q2. What is the name of the malicious file used for initial access?
The first HTTP request packet.
<img width="1504" height="364" alt="image" src="https://github.com/user-attachments/assets/434c9621-863c-49a9-aaa3-3d675dba7262" />

## Q3. What is the SHA-256 hash of the malicious file used for initial access?
847b4ad90b1daba2d9117a8e05776f3f902dda593fb1252289538acf476c4268
We got the hash by exporting login.php file to our windows and getting the hash.
## Q4. Which process was used to execute the malicious file?
We can get this by various methots. Either just by checking reports using the hash we got, or analyzing obfuscated js code that we have in the second HTTP packet. <img width="1235" height="782" alt="image" src="https://github.com/user-attachments/assets/3b65f715-63a6-499f-8695-d5c528e5a1f1" />
<img width="1261" height="238" alt="image" src="https://github.com/user-attachments/assets/6cbd458b-e04d-4430-b705-80c3e094028d" />
<img width="954" height="448" alt="image" src="https://github.com/user-attachments/assets/04746c2e-8a4b-424c-882b-b982f79b2535" />
wscript.exe
## Q5. What is the file extension of the second malicious file utilized by the attacker?
Answer is in the previous question. And also in one of the packets.
<img width="341" height="30" alt="image" src="https://github.com/user-attachments/assets/788d2356-0800-435a-9225-a7faa9e78904" />
.dll
## Q6. What is the MD5 hash of the second malicious file?
Export resources.dll and got the hash.
e758e07113016aca55d9eda2b0ffeebe

<img width="559" height="173" alt="image" src="https://github.com/user-attachments/assets/250e21e7-850f-4d14-88ef-15c5d6d3cdf9" />
