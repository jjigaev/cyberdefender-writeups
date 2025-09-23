# Reveal Lab. 
Reconstruct a multi-stage attack by analyzing Windows memory dumps using Volatility 3, identifying malicious processes, command lines, and correlating findings with threat intelligence.
For this lab we need to download Volatility3, in github repo.
# Scenario
You are a forensic investigator at a financial institution, and your SIEM flagged unusual activity on a workstation with access to sensitive financial data. Suspecting a breach, you received a memory dump from the compromised machine.Your task is to analyze the memory for signs of compromise, trace the anomaly's origin, and assess its scope to contain the incident effectively.

## Q1. Identifying the name of the malicious process helps in understanding the nature of the attack.
<img width="3418" height="218" alt="image" src="https://github.com/user-attachments/assets/ceea7832-33e2-44fe-bca5-118a4a163264" />
I used command python3 vol.py -f mem.dmp windows.pstree. To use command we need call python3 volatility script, add argument "-f" that leads to filename. Write a dump name, then write a needed volatility plugin. 

In this case, we used **pstree**, in some cases correcting with proper memory dump OS needed for proper work.
**pstree** displays all the processes in a tree form which makes it much easier for us to navigate.

It is quite suspicious that powershell.exe is running with a bunch of tasks under it. Most likely a sign of malware.
**Answer:** powershell.exe

## Q2. Knowing the parent process ID (PPID) of the malicious process aids in tracing the process hierarchy and understanding the attack flow.
**Answer:** 4120
From "pstree" output.

## Q3. Determining the file name used by the malware for executing the second-stage payload is crucial for identifying subsequent malicious activities.
**Answer:** 3435.dll
Review the command described in the answer for Q1 and you will see the answer.

<img width="436" height="67" alt="image" src="https://github.com/user-attachments/assets/01f7c752-db69-475e-a5e2-9b16b8e21c2d" />

## Q4. Identifying the shared directory on the remote server helps trace the resources targeted by the attacker.
**Answer:** davwwwroot
<img width="436" height="67" alt="image" src="https://github.com/user-attachments/assets/475c51ff-0b3f-498c-8d62-d963f4f8e5e2" />

## Q5. What is the MITRE ATT\&CK sub-technique ID that describes the execution of a second-stage payload using a Windows utility to run the malicious file?
**Answer:** T1218.011
I searched the MITRE website to find the ID associated with rundll32.

<img width="1659" height="684" alt="image" src="https://github.com/user-attachments/assets/eeff8a63-255c-4b1a-944c-26085335798f" />

## Q6. Identifying the username under which the malicious process runs helps in assessing the compromised account and its potential impact.
**Answer:** Elon
Windows stores each sessions, we can retrieve this data by volatility3 plugin. Using "windows.sessions" to display all sessions associated with each process which we can see that this PowerShell and other processes were executed with "Elon" account.

<img width="1140" height="47" alt="image" src="https://github.com/user-attachments/assets/e22c52a5-1ca6-4754-815d-64010b56a97f" />

## Q7. Knowing the name of the malware family is essential for correlating the attack with known threats and developing appropriate defenses. What is the name of the malware family?
**Answer:** StrelaStealer
Search the IP in VirusTotal.

<img width="1750" height="735" alt="image" src="https://github.com/user-attachments/assets/9b3931a7-0b4b-4905-9285-9ef51997148b" />



## Q7

Knowing the name of the malware family is essential for correlating the attack with known threats and developing appropriate defenses.
**Answer:** (malware family name not provided in the text)
