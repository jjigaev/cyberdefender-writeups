<img width="1308" height="318" alt="image" src="https://github.com/user-attachments/assets/834fdbf4-93cc-4726-8018-520ab63712ef" />
<img width="1047" height="579" alt="image" src="https://github.com/user-attachments/assets/33d13566-0e65-4268-a795-fc398cb7e476" />


We start this lab with a memory dump of Linux server that has been compromised. We need to investigate the incident and uncover the attacker’s activities.
In this assignment, we can use Volatility on Linux system or FTK imager on Windows system. <img width="112" height="93" alt="image" src="https://github.com/user-attachments/assets/3f8d269e-6213-4a99-b29a-009ae271ee34" />
We got:
victoria-v8.kcore.img
victoria-v8.memdump.img – memory dump
victoria-v8.sda1.img – image disk
I downloaded needed Volatility profile in Debian5_26.zip and then mounted image in FTK.

## Q1 The attacker was performing a Brute Force attack. What account triggered the alert?  
To investigate this, we need to look for any logs that shows logging into accounts. On the Linux Debian, it usually is **/var/log/auth.log**.
So lets look for this one on the FTK imager.
<img width="1214" height="353" alt="image" src="https://github.com/user-attachments/assets/88688c7d-f888-4211-8a86-f43d5db9c1e3" />
Answer: ulysses

## Q2 During investigating the logs. How many failed login attempts were alerted by the same user?  
I further anaylez auth.log file for failed login related to the previous identified user. <img width="1144" height="356" alt="image" src="https://github.com/user-attachments/assets/d9bbc745-918f-43b1-a2bd-3a86c43be7e0" />
Using powershell commands **strings** **Select-String** for Failed Ulysses events i counted 32 attempts.
Answer: 32

## Q3 What kind of system runs on the targeted server?  
I looked in *os-release* system file in /etc directory.
Answer:Debian GNU/Linux 5.0

## Q4 What is the victim's IP address?  
On this one, i used Volatility. Since my forensic tools is on my Linux system, i used Linux to perform next tasks. linux_netstat plugin showed this
<img width="1041" height="177" alt="image" src="https://github.com/user-attachments/assets/7c6fe0e9-272a-418d-b6ac-7ee330762837" />
Answer: 192.168.56.102

## Q5 What are the attacker's two IP addresses?  
From the previous step:
Answer: 192.168.56.1,192.168.56.101

## Q6 What is the nc service PID number that was running on the server?  
so nc in netcat - networking utility. It is usually used by attacker for setting backdoors or reverse shells.
From the linux_netstat output, we know the PID is 2169
Answer: 2169

## Q7 What service was exploited to gain access to the system?  
First, i checked for all bash command to see for any suspicious executions on system. <img width="743" height="453" alt="image" src="https://github.com/user-attachments/assets/94604e51-7450-41b9-bcf8-4799101f5aa7" />
We see a lot of activity involving exim4. We can further prove it by looking exim4 log files.
Answer: Exim4

## Q8 What is the CVE number of exploited vulnerability?  
Google this. In this case we will search for the buffer overflow attack using exim service.
Answer: CVE-2010-4344

## Q9 During this attack, the attacker downloaded two files to the server. Provide the name of the compressed file.  
Since exim4 is used to access. We will analyze exim4 log files, particularly /mainlog files shows *wget* commands appear.
Wget command downloads files to /tmp. Examining /tmp we see suspicious archive file **rk.tar**. Then i checked it by grepping for rk.tar in /mainlog.
<img width="395" height="22" alt="image" src="https://github.com/user-attachments/assets/baa7ff4a-9edf-4502-b0f0-233ed182fd49" />
We couldve done it faster by just using *string* command for /mainlog file.
Answer: rk.tar

## Q10 During the investigation, two ports were involved in the process of data exfiltration. Which port did the nc command used for the exfiltration?  
<img width="565" height="33" alt="image" src="https://github.com/user-attachments/assets/9e298334-7d6b-4726-9982-ce2ded2bbcf3" />

Answer: 8888

## Q11 Which port did the attacker try to block on the firewall?  
Extract .tar file. We analyze the extracted file, particularly the scripts because script file .sh contains commands to execution. 
I found install.sh file. Within the script, this lines were identified: 
echo "/usr/sbin/iptables -I OUTPUT 1 -p tcp --dport 45295 -j DROP" >> /etc/rc.d/rc.local
echo "/usr/sbin/iptables -I OUTPUT 1 -p tcp --dport 45295 -j DROP" >> /etc/init.d/xfs3
iptables -I OUTPUT 1 -p tcp --dport 45295 -j DROP
Answer: 45295
<img width="549" height="98" alt="image" src="https://github.com/user-attachments/assets/84c7e6d2-0b9c-4210-8c46-2139d741b63c" />
