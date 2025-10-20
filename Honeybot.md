<img width="1262" height="212" alt="image" src="https://github.com/user-attachments/assets/082626bf-f759-45b3-98e0-15d0a742d6e0" />
# Scenario
A PCAP analysis exercise highlighting attacker's interactions with honeypots and how automatic exploitation works.. (Note that the IP address of the victim has been changed to hide the true location.)
As a soc analyst, analyze the artifacts and answer the questions.

## Q1. What is the attacker's IP address?
Look up >Conversations tab.
<img width="1642" height="114" alt="image" src="https://github.com/user-attachments/assets/7a304fd5-4217-48c9-be63-23651b1a9546" />
<img width="1918" height="307" alt="image" src="https://github.com/user-attachments/assets/fd3d1764-6982-4eb7-9712-7ba9391a8705" />
As we see, this IP first tries to connect.
Answer: 98.114.205.102 
## Q2. What is the target's IP address?
<img width="405" height="58" alt="image" src="https://github.com/user-attachments/assets/4d9f6f4e-6aff-447c-bceb-baeadabf428f" />
Answer: 192.150.11.111
## Q3. Provide the country code for the attacker's IP address (a.k.a geo-location).
<img width="1095" height="369" alt="image" src="https://github.com/user-attachments/assets/e12c6c34-e5ba-4c16-b638-78cf8c762b76" />
Answer: US
## Q4. How many TCP sessions are present in the captured traffic?
<img width="259" height="175" alt="image" src="https://github.com/user-attachments/assets/570005ac-4046-4080-bdba-d0e00367b011" />
Look up >Statitistics tab and TCP sessions.
Answer: 5
## Q5. How long did it take to perform the attack (in seconds)?
<img width="450" height="47" alt="image" src="https://github.com/user-attachments/assets/03c719d8-c945-40bc-8037-49248177f059" />
<img width="1590" height="48" alt="image" src="https://github.com/user-attachments/assets/72f7195b-6124-4092-89d1-0dedb70a8c85" />
Answer: 16 seconds
## Q6. Provide the CVE number of the exploited vulnerability.
<img width="1537" height="450" alt="image" src="https://github.com/user-attachments/assets/1b8974ee-8e30-4e31-8e23-a5598ea7a8c3" />
DCERPC (Distributed Computing Environment / Remote Procedure Call), which is used for remote management and execution of services on Windows systems.
This protocol is often exploited in attacks targeting Microsoft Windows, particularly when used in combination with SMB (Server Message Block).
<img width="828" height="281" alt="image" src="https://github.com/user-attachments/assets/a74e140d-bdb8-4210-8c49-068c1c876288" />
Answer: CVE-2003-0533
## Q7. Which protocol was used to carry over the exploit?
Answer: SMB
As the previous answers suggested.
## Q8. Which protocol did the attacker use to download additional malicious files to the target system?
The protocol was used to download the malicious file. So we will look for TCP stream.
<img width="1143" height="170" alt="image" src="https://github.com/user-attachments/assets/cf8d401e-facc-499a-85d9-7d0107ede6ad" />
This FTP protocol. ftp -n -s:o line confirms it.
Answer: FTP
## Q9. What is the name of the downloaded malware?
Answer: ssms.exe
## Q10. The attacker's server was listening on a specific port. Provide the port number.
Based on the previous command analysis, whis is non standart FTP port.
Answer: 8884
## Q11. When was the involved malware first submitted to VirusTotal for analysis? Format: YYYY-MM-DD
In this case, we wiil extract the hash of TCP unknown packet. <img width="1011" height="35" alt="image" src="https://github.com/user-attachments/assets/ae174916-392e-43e6-997e-2aefb39a1d70" />
<img width="476" height="81" alt="image" src="https://github.com/user-attachments/assets/2d36966f-5bab-4d54-ac09-2418fd9cc062" />
<img width="225" height="38" alt="image" src="https://github.com/user-attachments/assets/0c070175-a5d5-4091-b2d8-46bb937f169c" />

Answer: 2007-06-27
## Q12. What is the key used to encode the shellcode?
We know that it was a buffer overflow attack. A buffer overflow attack occurs when an attacker sends more data than the allocated memory buffer can handle, leading to unintended code execution. 
<img width="1074" height="266" alt="image" src="https://github.com/user-attachments/assets/e305ca54-2e57-4717-a4e1-7fd08bf3fac9" />
We can see the process. Thus, the shellcode must be between frame 28 and 33. 
In 29, we see the needed packet. <img width="951" height="858" alt="image" src="https://github.com/user-attachments/assets/c77c6e37-a8b8-4359-bf6d-4574e38c554c" />
Now lets save it and examine using **scdbg**
<img width="492" height="84" alt="image" src="https://github.com/user-attachments/assets/1607e6ba-2ebb-40b1-aad1-7adb567b90c4" />
Answer: 0x99
## Q13. What is the port number the shellcode binds to?
<img width="422" height="56" alt="image" src="https://github.com/user-attachments/assets/7d0e0181-0a6b-44ab-a9da-eaaee30e9036" />
Answer: 1957
It is essentially a backoor.
## Q14. The shellcode used a specific technique to determine its location in memory. What is the OS file being queried during this process?
<img width="349" height="199" alt="image" src="https://github.com/user-attachments/assets/e111fe82-5b7c-432a-9edf-c58e974b7e25" />
It uses multiple GetProcAddress, which is a function of Kernel32.dll.
Answer: Kernel32.dll.

<img width="560" height="278" alt="image" src="https://github.com/user-attachments/assets/5c904115-020d-431f-9461-a8dad2231b7e" />
