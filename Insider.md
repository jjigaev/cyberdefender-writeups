<img width="1294" height="298" alt="image" src="https://github.com/user-attachments/assets/6f614e1f-7351-4667-aa8a-f1a63bb0847c" />
<img width="1112" height="301" alt="image" src="https://github.com/user-attachments/assets/69d7b7da-364a-4832-9746-25e80780fd30" />

## Q1 Which Linux distribution is being used on this machine?  
To look for any system info, we will go to the log files.
<img width="1160" height="560" alt="image" src="https://github.com/user-attachments/assets/9e98d1b3-60ad-4fdc-922a-2b6af46f228e" />
Syslog file that logs system activities, error states, and security events. 
Answer: kali

## Q2 What is the MD5 hash of the Apache access.log file?  
So we need to navigate to Apache files. Since we knew that the system is Kali Linux, we can easily find the proper location of files.
The Apache logs are typically located in /var/log/apache2/. Once i found the access.log file, i exported file and extracted the hash.<img width="845" height="155" alt="image" src="https://github.com/user-attachments/assets/89435bf1-d99d-4332-a736-f184bd33cafb" />
Answer: d41d8cd98f00b204e9800998ecf8427e

## Q3 It is suspected that a credential dumping tool was downloaded. What is the name of the downloaded file?  
Downloads directory. 
<img width="848" height="47" alt="image" src="https://github.com/user-attachments/assets/97b64126-95e9-4827-91b5-63bc81b5b7e7" />

Answer: mimikatz_trunk.zip

## Q4 A super-secret file was created. What is the absolute path to this file?  
Lets look for the bash history. 
<img width="952" height="708" alt="image" src="https://github.com/user-attachments/assets/fbe1042e-c7d1-4988-bf58-52f14ec5afcf" />

Answer: /root/Desktop/SuperSecretFile.txt

## Q5 What program used the file didyouthinkwedmakeiteasy.jpg during its execution?  
Same in the bash history.
<img width="488" height="39" alt="image" src="https://github.com/user-attachments/assets/0f6d757e-2faf-4278-b674-2d032801dbe7" />
Binwalk - forensic tool used to inspect images, executables, and other binary data for hidden payloads or embedded code.
Answer: binwalk

## Q6 What is the third goal from the checklist Karen created?  
look up /Desktop. 
<img width="428" height="231" alt="image" src="https://github.com/user-attachments/assets/d50d6ff8-99d2-4a38-9ff1-4eafbf3843f4" />

Answer: profit

## Q7 How many times was Apache run?  
Lets look apache2 logs.
<img width="542" height="110" alt="image" src="https://github.com/user-attachments/assets/b29fd9de-b1d8-43d6-a452-12adea8aa62e" />
All logs contains nothing, so there is no running apache happened.
Answer: 0

## Q8 This machine was used to launch an attack on another. Which file contains the evidence for this?  
The root directory might contain files that provide evidence of malicious activities. Lets look for any suspicious files there.
<img width="1689" height="712" alt="image" src="https://github.com/user-attachments/assets/4ba3a00d-9d38-4022-a788-0613704f2cbf" />

Answer: irZLAohL.jpeg

## Q9 It is believed that Karen was taunting a fellow computer expert through a bash script within the Documents directory. Who was the expert that Karen was taunting?  
<img width="1222" height="548" alt="image" src="https://github.com/user-attachments/assets/1e1310aa-7d83-43d7-83e4-fdd4e9b94c5b" />

Answer: Young

## Q10 A user executed the su command to gain root access multiple times at 11:26. Who was the user?  
Lets look systems authentication logs. In the case Kali Linux, it is /var/log/auth.log. 
We will look for user that sudoed to root at 11:26 multiple times.
<img width="992" height="440" alt="image" src="https://github.com/user-attachments/assets/bdaf63e7-89ec-47b4-b501-007486418157" />

Answer: postgres

## Q11 Based on the bash history, what is the current working directory?  
<img width="289" height="129" alt="image" src="https://github.com/user-attachments/assets/11d4d66b-1769-4c8c-9033-4d189c0b2e5c" />

Answer: /root/Documents/myfirsthack/

<img width="590" height="155" alt="image" src="https://github.com/user-attachments/assets/6e4e9cb3-864b-4984-967a-2dda4ae248f8" />
