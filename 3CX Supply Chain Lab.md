<img width="1255" height="286" alt="image" src="https://github.com/user-attachments/assets/b1e5585f-24a5-4b71-9e42-f235df1bce0e" />
Given a 3CXDesktopApp-18.12.416.msi file for analysis.

# Scenario
A large multinational corporation heavily relies on the 3CX software for phone communication, making it a critical component of their business operations. After a recent update to the 3CX Desktop App, antivirus alerts flag sporadic instances of the software being wiped from some workstations while others remain unaffected. 
Dismissing this as a false positive, the IT team overlooks the alerts, only to notice degraded performance and strange network traffic to unknown servers. Employees report issues with the 3CX app, and the IT security team identifies unusual communication patterns linked to recent software updates.

As the threat intelligence analyst, it's your responsibility to examine this possible supply chain attack. 
Your objectives are to uncover how the attackers compromised the 3CX app, identify the potential threat actor involved, and assess the overall extent of the incident. 

## What Is a Supply Chain Attack?
A supply chain attack is a sophisticated tactic where adversaries target the trusted services or software a victim depends on, rather than striking the victim directly. 
In the 3CX incident, the attackers weaponized the company’s own update mechanism, transforming a routine software update into a stealthy Trojan horse. 3CX is a major provider of unified communications software.

## Q1. Understanding the scope of the attack and identifying which versions exhibit malicious behavior is crucial for making informed decisions if these compromised versions are present in the organization. How many versions of 3CX running on Windows have been flagged as malware?
Begin by consulting public threat intelligence reports, which document the affected builds, associated indicators of compromise (IOCs), and observed attacker tactics. In this case, we searched several reports and found this:
<img width="574" height="146" alt="image" src="https://github.com/user-attachments/assets/2c49529d-92a4-46a4-bd9d-53802f375923" />
**Answer:** 2

## Q2. Determining the age of the malware can help assess the extent of the compromise and track the evolution of malware families and variants. What's the UTC creation time of the .msi malware?
We will get the hash from the report since its faster.
<img width="1122" height="120" alt="image" src="https://github.com/user-attachments/assets/543de8f5-55e7-4e5d-871b-54b4da1fbd2e" />
We go to Virus Total, to look for report using the hash we gained from report.
<img width="931" height="266" alt="image" src="https://github.com/user-attachments/assets/423fae81-a5fc-42ee-91e2-579a9c099668" />
<img width="608" height="39" alt="image" src="https://github.com/user-attachments/assets/7aa28e9e-f61c-4543-a92a-2ac279883bcd" />
**Answer:** 2023-03-13 06:33

## Q3. Executable files (.exe) are frequently used as primary or secondary malware payloads, while dynamic link libraries (.dll) often load malicious code or enhance malware functionality. Analyzing files deposited by the Microsoft Software Installer (.msi) is crucial for identifying malicious files and investigating their full potential. Which malicious DLLs were dropped by the .msi file?
In VirusTotal, under the Relations tab > Bundled Files, We can find the two DLL libraries.
<img width="1123" height="92" alt="image" src="https://github.com/user-attachments/assets/10911bbd-6310-40aa-ba76-ede7137d4e77" />
**Answer:** ffmpeg.dll, d3dcompiler_47.dll

## Q4. Recognizing the persistence techniques used in this incident is essential for current mitigation strategies and future defense improvements. What is the MITRE Technique ID employed by the .msi files to load the malicious DLL?
<img width="1244" height="363" alt="image" src="https://github.com/user-attachments/assets/d23ba1c8-5966-46db-b465-db3e07b9e7e5" />
<img width="1024" height="241" alt="image" src="https://github.com/user-attachments/assets/9e08cc6c-9ff5-4f48-b7b1-36ce3a90b344" />
**Answer:** T1574

## Q5. Recognizing the malware type (threat category) is essential to your investigation, as it can offer valuable insight into the possible malicious actions you'll be examining. What is the threat category of the two malicious DLLs?
<img width="423" height="76" alt="image" src="https://github.com/user-attachments/assets/8e1bf712-e611-4c0f-baec-e42aa23eed09" />
**Answer:** Trojan

## Q6. As a threat intelligence analyst conducting dynamic analysis, it's vital to understand how malware can evade detection in virtualized environments or analysis systems. This knowledge will help you effectively mitigate or address these evasive tactics. What is the MITRE ID for the virtualization/sandbox evasion techniques used by the two malicious DLLs?
<img width="1036" height="243" alt="image" src="https://github.com/user-attachments/assets/b36fcfb2-c4c2-42ef-a4c3-e0624dc58bd5" />
**Answer:** T1497

## Q7. When conducting malware analysis and reverse engineering, understanding anti-analysis techniques is vital to avoid wasting time. Which hypervisor is targeted by the anti-analysis techniques in the ffmpeg.dll file?
In the information for the ffmpeg.dll library on VirusTotal, under the Behavior tab here is a reference indicating that it is anti-VMware. But i could find any information about this.
I guessed it coulde be because of the different hash I used in VirusTotal. To find this information, I extracted the hash of the MSI file and search for it on VirusTotal. <img width="913" height="298" alt="image" src="https://github.com/user-attachments/assets/0b49971e-459e-4de4-954b-5e0522f73ea2" />
<img width="1052" height="155" alt="image" src="https://github.com/user-attachments/assets/e87768a7-594a-43d4-96ce-45a50056d5cc" />

**Answer:** VMware

## Q8. Identifying the cryptographic method used in malware is crucial for understanding the techniques employed to bypass defense mechanisms and execute its functions fully. What encryption algorithm is used by the ffmpeg.dll file?
On this one, look up for "Behavior" tab. On this tab, we can look for different techniques of the malware.
<img width="542" height="371" alt="image" src="https://github.com/user-attachments/assets/308d02e5-82b5-4004-b1fa-195d9ece57fb" />
**Answer:** RC4

## Q9. As an analyst, you've recognized some TTPs involved in the incident, but identifying the APT group responsible will help you search for their usual TTPs and uncover other potential malicious activities. Which group is responsible for this attack?
This was hard one, because several reports showed name of another group "CHOLLIMA". But then, i researched this group deeper and found out that it had other name.
<img width="831" height="148" alt="image" src="https://github.com/user-attachments/assets/9fdefa8e-43b3-4f3a-b38a-8300c9dad281" />

**Answer:** Lazarus
##
Now, lab is completed.<img width="542" height="110" alt="image" src="https://github.com/user-attachments/assets/6d2a5a2c-8511-4b9f-8092-6d3d9b06c38d" />
Thata all for today.
