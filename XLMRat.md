# Scenario
A compromised machine has been flagged due to suspicious network traffic. Your task is to analyze the PCAP file to determine the attack method, identify any malicious payloads, and trace the timeline of events. 
Focus on how the attacker gained access, what tools or techniques were used, and how the malware operated post-compromise.

## Q1. The attacker successfully executed a command to download the first stage of the malware. What is the URL from which the first malware stage was installed?
<img width="1912" height="668" alt="image" src="https://github.com/user-attachments/assets/cfbb5075-c464-45c2-8b19-39e9e8ee52c4" />
http://45.126.209.4:222/mdm.jpg
The first download was xlm.txt file that contained obfuscated malicious code designed to execute hidden PowerShell command. From which we can tell that second mdm.jpg payload contained malware. If we deobfucate this code, 
we find that code downloads mdm.jpg <img width="926" height="85" alt="image" src="https://github.com/user-attachments/assets/50931a9b-c584-4377-8add-09cbfe589427" />

## Q2. Which hosting provider owns the associated IP address?
<img width="995" height="589" alt="image" src="https://github.com/user-attachments/assets/7b77d9e7-5dc8-4020-b12b-d0cdc8157ef4" />


## Q3. By analyzing the malicious scripts, two payloads were identified: a loader and a secondary executable. What is the SHA256 of the malware executable?
I extracted the hexadecimal string from the payload. I then used CyberChef for the analysis. <img width="1871" height="649" alt="image" src="https://github.com/user-attachments/assets/c49902ae-9b54-473a-ab5b-c7db4328c3cd" />

<img width="1910" height="693" alt="image" src="https://github.com/user-attachments/assets/eb76a739-88e2-418d-a5c9-2086efbef4a6" />

## Q4. What is the malware family label based on Alibaba?
Go to VirusTotal with hash. <img width="213" height="48" alt="image" src="https://github.com/user-attachments/assets/d780dbf7-3a32-4919-a638-f44ebc8b2b04" />

## Q5. What is the timestamp of the malware's creation?
2023-10-30 15:08
<img width="577" height="70" alt="image" src="https://github.com/user-attachments/assets/0df69409-0752-420d-b4fd-d4394cc2dbdc" />

## Q6. Which LOLBin is leveraged for stealthy process execution in this script? Provide the full path.

To find this, we go to payload packet mdm.jpg and analyse the code.
<img width="961" height="278" alt="image" src="https://github.com/user-attachments/assets/542a5750-26dd-43f1-b4b7-587104a50644" />
The powershell is very obfuscated, lets deobfuscate it to retrieve full path.<img width="679" height="33" alt="image" src="https://github.com/user-attachments/assets/3f502c8c-7629-4417-b83e-b63c56ca5749" />
C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe

## Q7. The script is designed to drop several files. List the names of the files dropped by the script.
<img width="612" height="57" alt="image" src="https://github.com/user-attachments/assets/cb2d0446-1bd2-4892-b677-943c31666c10" />
<img width="679" height="67" alt="image" src="https://github.com/user-attachments/assets/583371ec-a45d-4784-95c7-88a4ede2289d" />
<img width="467" height="117" alt="image" src="https://github.com/user-attachments/assets/85489f52-79e5-4d0b-a274-283d26f27c03" />

<img width="638" height="121" alt="image" src="https://github.com/user-attachments/assets/fd0922ab-a6f6-4765-afb5-ba2415bc43e6" />
