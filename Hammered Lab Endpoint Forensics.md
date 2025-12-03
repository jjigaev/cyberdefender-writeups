<img width="1299" height="304" alt="image" src="https://github.com/user-attachments/assets/e8927c4c-5ab3-43d5-9a61-699f99d307a8" />
<img width="1141" height="279" alt="image" src="https://github.com/user-attachments/assets/ce817580-8fd3-4a02-9ade-57b82456cdd8" />
We are given a zip file and a list of files to focus our analysis on.

## Q1. Which service did the attackers use to gain access to the system?
Check system logs for authentication failures. These logs can help you identify the service attackers tried to use to gain access.
```
grep "fail" auth.log | less
```

``` 
pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=8.12.45.242  user=root
pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=8.12.45.242  user=root
pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=8.12.45.242  user=root
```
Answer: SSH
## Q2. What is the operating system version of the targeted system?
Lets check system logs for this information.

```
grep "version" kern.log
```

```
kernel: [    0.000000] Linux version 2.6.24-26-server (buildd@crested) (gcc version 4.2.4 (Ubuntu 4.2.4-1ubuntu3)) #1 SMP Tue Dec 1 18:26:43 UTC 2009 (Ubuntu 2.6.24-26.64-server)
```
Answer: 4.2.4-1ubuntu3
## Q3. What is the name of the compromised account?
From 1st question it gives root.
Answer: root
## Q4. How many attackers, represented by unique IP addresses, were able to successfully access the system after initial failed attempts?
Lets see successful login attempts from different IP addresses in the auth logs.
```bash
cat auth.log| grep "Accepted password" | awk '{print $9,$11}' | sort | uniq -c | sort -n
1 fido 94.52.185.9
1 root 10.0.1.2
1 root 151.81.204.141
1 root 151.81.205.100
1 root 151.82.3.201
1 root 188.131.22.69
1 root 190.167.70.87
1 root 190.167.74.184
1 root 193.1.186.197
1 root 201.229.176.217
1 root 222.169.224.197
1 root 222.66.204.246
1 root 61.168.227.12
1 root 94.52.185.9
1 user1 166.129.196.88
1 user1 208.80.69.70
1 user1 65.195.182.120
1 user1 67.164.72.181
1 user3 208.80.69.74
1 user3 65.195.182.120
2 dhg 190.167.74.184
2 root 121.11.66.70
2 root 122.226.202.12
2 user3 208.80.69.69
3 root 190.166.87.164
3 user3 192.168.126.1
4 root 188.131.23.37
4 root 219.150.161.20
4 user3 10.0.1.4
5 user2 71.132.129.212
6 user1 208.80.69.74
6 user1 65.88.2.5
13 user3 10.0.1.2
20 dhg 190.166.87.164
22 user1 76.191.195.140
```

awk '{print $9,$11}' allows to get names of users and IP. In this case we see the use of different IP addresses.
Answer:6 
## Q5. Which attacker's IP address successfully logged into the system the most number of times?
```
cat auth.log| grep "Accepted password" | grep root | awk '{print $11}' | sort | uniq -c | sort -n

1 10.0.1.2  
1 151.81.204.141  
1 151.81.205.100  
1 151.82.3.201  
1 188.131.22.69  
1 190.167.70.87  
1 190.167.74.184  
1 193.1.186.197  
1 201.229.176.217  
1 222.169.224.197  
1 222.66.204.246  
1 61.168.227.12  
1 94.52.185.9  
2 121.11.66.70  
2 122.226.202.12  
3 190.166.87.164  
4 188.131.23.37  
4 219.150.161.20  
```
## Q6. How many requests were sent to the Apache Server?
We need to analyze the Apache servers access logs to find the number of requests. 
The Apache server logs are stored in the /var/log/apache2/ directory (or a similar path, depending on the system). We will look to access logs
```
cat www-access.log |wc -l
```
Answer: 365
## Q7. How many rules have been added to the firewall?
This kinda entries is usually seen on the "iptables". In this case we will look for all rules that were "allowed" in the Firewall.

```
cat auth.log | grep iptablec
Apr 15 12:49:00 app-1 sudo: user1 : TTY=pts/0 ; PMD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee .../templates/proxy/ptables.conf
Apr 15 15:06:13 app-1 sudo: user1 : TTY=pts/1 ; PMD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee .../templates/proxy/ptables.conf
Apr 15 15:17:45 app-1 sudo: user1 : TTY=pts/1 ; PMD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee .../templates/proxy/ptables.conf
Apr 15 15:18:23 app-1 sudo: user1 : TTY=pts/1 ; PMD=/opt/software/web/app ; USER=root ; COMMAND=/usr/bin/tee .../templates/proxy/ptables.conf
Apr 24 19:25:37 app-1 sudo: root : TTY=pts/2 ; PMD=/etc ; USER=root ; COMMAND=/sbin/ptables -L
Apr 24 20:03:06 app-1 sudo: root : TTY=pts/2 ; PMD=/etc ; USER=root ; COMMAND=/sbin/ptables -A INPUT -p ssh -dport 2424 -j ACCEPT
Apr 24 20:03:44 app-1 sudo: root : TTY=pts/2 ; PMD=/etc ; USER=root ; COMMAND=/sbin/ptables -A INPUT -p tcp -dport 53 -j ACCEPT
Apr 24 20:04:13 app-1 sudo: root : TTY=pts/2 ; PMD=/etc ; USER=root ; COMMAND=/sbin/ptables -A INPUT -p udp -dport 53 -j ACCEPT
Apr 24 20:06:22 app-1 sudo: root : TTY=pts/2 ; PMD=/etc ; USER=root ; COMMAND=/sbin/ptables -A INPUT -p tcp --dport ssh -j ACCEPT
Apr 24 20:11:00 app-1 sudo: root : TTY=pts/2 ; PMD=/etc ; USER=root ; COMMAND=/sbin/ptables -A INPUT -p tcp --dport 53 -j ACCEPT
Apr 24 20:11:08 app-1 sudo:
```
## Q8. One of the downloaded files on the target system is a scanning tool. What is the name of the tool?
/var/log/apt/term.log is exact path for log files that concludes this info.
Answer: nmap
## Q9. When was the last login from the attacker with IP 219.150.161.20? Format: MM/DD/YYYY HH:MM:SS AM
Lets again examine auth.log but with more narrowing down cause we know some details so far.
For this question, we grep the timestamp to get the time and date.
```
grep 04:43:15 auth.log
```
<img width="1152" height="59" alt="image" src="https://github.com/user-attachments/assets/d096e915-862b-4597-a42e-1431e17c5f24" />
Answer: 2010-04-19 05:56
## Q10. The database showed two warning messages. Please provide the most critical and potentially dangerous one.
Such warnings are logged in system logs as daemon.log.
```
cat daemon.log | grep -1 warning

Mar 18 10:18:42 app-1 /etc/mysql/debian-start[7566]:    MARXING: mysql.user contains 2 root accounts without password!
Mar 18 17:01:44 app-1 /etc/mysql/debian-start[14717]:    MARXING: mysql.user contains 2 root accounts without password!
Mar 22 13:49:49 app-1 /etc/mysql/debian-start[5599]:    MARXING: mysql.user contains 2 root accounts without password!
Mar 22 18:43:41 app-1 /etc/mysql/debian-start[4755]:    MARXING: mysql.user contains 2 root accounts without password!
Mar 22 18:45:25 app-1 /etc/mysql/debian-start[4749]:    MARXING: mysql.user contains 2 root accounts without password!
Mar 25 11:56:53 app-1 /etc/mysql/debian-start[4848]:    MARXING: mysql.user contains 2 root accounts without password!
Apr 14 14:44:34 app-1 /etc/mysql/debian-start[5369]:    MARXING: mysql.user contains 2 root accounts without password!
Apr 14 14:44:36 app-1 /etc/mysql/debian-start[5624]:    MARXING: mysql.check has found corrupt tables
Apr 18 18:04:00 app-1 /etc/mysql/debian-start[4647]:    MARXING: mysql.user contains 2 root accounts without password!
Apr 24 20:20:52 app-1 collection[4971]:    MARXING: collected was terminated by signal 11
Apr 24 20:21:24 app-1 /etc/mysql/debian-start[5427]:    MARXING: mysql.user contains 2 root accounts without password!
Apr 28 07:34:26 app-1 /etc/mysql/debian-start[4782]:    MARXING: mysql.user contains 2 root accounts without password!
Apr 28 07:34:27 app-1 /etc/mysql/debian-start[5032]:    MARXING: mysql.check has found corrupt tables
Apr 28 07:34:27 app-1 /etc/mysql/debian-start[5032]:    MARXING: : 1 client is using or hasn't closed the table properly
Apr 28 09:35:31 app-1 collection[5119]:    MARXING: collected was terminated by signal 11
May  2 23:05:54 app-1 /etc/mysql/debian-start[4774]:    MARXING: mysql.user contains 2 root accounts without password!
```
## Q11. Multiple accounts were created on the target system. Which account was created on April 26 at 04:43:15?
Lets just use the method like in question 9.
<img width="1138" height="24" alt="image" src="https://github.com/user-attachments/assets/e8d097ad-a0de-45e6-8226-15d4421b8cce" />

## Q12. Few attackers were using a proxy to run their scans. What is the corresponding user-agent used by this proxy?
Since the question is about proxy, we need to look for network. Lets examine web server, precisely access logs for unusual user-agent strings, as these can reveal the use of a proxy or scanning tool.
apache2/www-access.log is target
```
cat www-access.log | cut -f 12 | sort | uniq #-f 12 because in apache logs 12 lines contains User-Agent
"Apple-PubSub/65.12.1"
"Mozilla/4.0
"Mozilla/5.0
"pxyscand/2.1"
"WordPress/2.9.2;
```

<img width="554" height="156" alt="image" src="https://github.com/user-attachments/assets/30b4af5b-bafe-430e-9bfe-a5ce01fbd596" />
