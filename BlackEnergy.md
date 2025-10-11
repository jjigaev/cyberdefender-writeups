<img width="1295" height="345" alt="image" src="https://github.com/user-attachments/assets/14081833-b193-4223-9b66-0627721cd167" />

# Scenario
A multinational corporation has suffered a cyber attack, resulting in the theft of sensitive data. 
The attack employed a previously unseen variant of the BlackEnergy v2 malware. 
The company's security team has obtained a memory dump from the infected machine and is seeking your expertise as a SOC analyst to analyze the dump in order to understand the scope and impact of the attack.

## Q1. Which volatility profile would be best for this machine?  
If we run the **imageinfo** command on the memory dump file that we gained. We would get the needed information.
*vol.py -f CYBERDEF-567078-20230213-171333.raw imageinfo*
<img width="459" height="25" alt="image" src="https://github.com/user-attachments/assets/08616fe3-3f6b-4213-86c1-724cbaeefe64" />

## Q2. How many processes were running when the image was acquired?  
Use pslist
19
## Q3. What is the process ID of cmd.exe?  
1960
## Q4. What is the name of the most suspicious process?  
rootkit.exe is very suspicious name.
## Q5. Which process shows the highest likelihood of code injection?  
In this case i used **malfind** plugin, its a plugin for collecting any suspicious activities on the memory dump.
It scans for memory regions within processes that have the characteristics of being both writable and executable, which is unusual under normal circumstances and often a sign of injected code.
In this image, svchost has suspicious output compared to other processes like csrss.exe and winlogon.exe that plugin analyzed.
<img width="719" height="145" alt="image" src="https://github.com/user-attachments/assets/7debe589-4379-4cb0-9dd3-7623bf2f5a67" />
You even can dump this file by PID and check for reports on the internet.
## Q6. There is an odd file referenced in the recent process. Provide the full path of that file.  
I iniatilly tried to look by **handles** command. It shows all open handles of a given process or all processes, showing references to system resources such as files, registry keys, events, and threads.
<img width="1445" height="439" alt="image" src="https://github.com/user-attachments/assets/17adf47b-e8c5-4bff-ad05-c2e685f22435" />

C:\WINDOWS\system32\drivers\str.sys
str.sys is not known system driver, so it is very highly suspicious.

## Q7. What is the name of the injected DLL file loaded from the recent process?  

Since we are looking for DLL files, Volatiliy has **lrpmodules** plugin that shows all loaded DLLs. 
<img width="1266" height="88" alt="image" src="https://github.com/user-attachments/assets/372f90d8-076d-40ce-88ef-f63b742035d5" />
We see a DLL named msxml3r.dll , which is suspicious because process in unlinked from process execution that seems like evasion technique.
## Q8. What is the base address of the injected DLL?
0x980000<img width="405" height="21" alt="image" src="https://github.com/user-attachments/assets/cc77a0c2-b666-4b2a-8296-1a75e8c0114e" />

Task is completed. <img width="540" height="142" alt="image" src="https://github.com/user-attachments/assets/167d9644-0ad6-4f98-865f-9d3bfff93ad1" />
