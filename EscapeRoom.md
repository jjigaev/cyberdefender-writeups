<img width="1254" height="219" alt="image" src="https://github.com/user-attachments/assets/ad39fe5a-1941-4c45-a6bf-cffe740b0aa2" />
<img width="1077" height="528" alt="image" src="https://github.com/user-attachments/assets/52e3f586-55ff-47de-9907-88249a04f9f4" />


## Q1: What service did the attacker use to gain access to the system?  
Lets go to 'Statistics > Protocol Hierarchy.' 
This view will give us a breakdown of all the protocols seen in the capture. 
<img width="1470" height="155" alt="image" src="https://github.com/user-attachments/assets/5c4f8ba9-e89e-4fd3-93a7-6871f117fd05" />
We see SSH connection.
Answer:SSH
## Q2: What attack type was used to gain access to the system? (one word)  
<img width="1611" height="770" alt="image" src="https://github.com/user-attachments/assets/6cb518f5-154b-441e-8a7d-213a02a8f54b" />
As we see a lot of SSH packets trying to bruteforce the key, its clear that it is bruteforce attack.
Answer: bruteforce
## Q3: What was the tool the attacker possibly used to perform this attack?  
As wee see the SSH packets, the tool performs bruteforce attacks using SSH. 
The most common tools is Hydra.
Answer: Hydra
## Q4: How many failed attempts were there?  
To see clearly statistics we will use ZUI.
<img width="1378" height="371" alt="image" src="https://github.com/user-attachments/assets/966c5cb5-2ce9-46dd-b7d9-ed259564f4e9" />
Answer: 52
## Q5: What credentials (username:password) were used to gain access? Refer to shadow.log and sudoers.log.  
To crack hash we will use John the Ripper as lab suggests.
<img width="310" height="22" alt="image" src="https://github.com/user-attachments/assets/b0b39a68-e072-46ce-b302-4d002ef2c5cc" />
Answer: manager:forgot
## Q6: What other credentials (username:password) could have been used to gain access also have SUDO privileges? Refer to shadow.log and sudoers.log.  
Answer: sean:spectre
## Q7: What is the tool used to download malicious files on the system?  
<img width="1114" height="499" alt="image" src="https://github.com/user-attachments/assets/00d5071a-4006-4b21-a0a1-e2c0ce96d28a" />
Answer: wget
## Q8: How many files did the attacker download to perform malware installation?  
<img width="970" height="361" alt="image" src="https://github.com/user-attachments/assets/91c0c9fd-b46a-4911-b2a0-0cbe9f60b6f5" />
Before malware payload packets, we see the 3 packets.
Answer: 3
## Q9: What is the main malware MD5 hash?  
Lets export this 3 files and take the hash. Then go to Virustotal to examine.
<img width="1634" height="825" alt="image" src="https://github.com/user-attachments/assets/1edf9d68-c2d5-41cf-85ba-b6f9d82754cd" />
Answer:772b620736b760c1d736b1e6ba2f885b
## Q10: What file has the script modified so the malware will start upon reboot?  
Examine HTTP packet, obviously the last one would be preferable because logic of the script.
<img width="1142" height="695" alt="image" src="https://github.com/user-attachments/assets/389170c7-fd26-473e-ab90-dbdb63fe2cb8" />
Answer: /etc/rc.local
## Q11: Where did the malware keep local files?  
<img width="897" height="272" alt="image" src="https://github.com/user-attachments/assets/b90fd1bd-ba67-4e57-a79b-967200869bc9" />
Based on the script, the malware binary /var/mail/mail is executed and likely stores local files in /var/mail/.
Answer: /var/mail/
## Q12: What is missing from ps.log?  
<img width="920" height="725" alt="image" src="https://github.com/user-attachments/assets/9d19a9d1-2502-4238-9671-228e5c264d44" />
Based on the earlier script and the ps.log output, the malware process /var/mail/ is missing from the process list even though the script set it to run at boot via /etc/rc.local.
## Q13: What is the main file that used to remove this information from ps.log?  
<img width="373" height="29" alt="image" src="https://github.com/user-attachments/assets/34ea8c47-70df-461b-b2a7-00a2f57ae227" />
Answer: sysmod.ko
## Q14: Inside the Main function, what is the function that causes requests to those servers?  
Now we need to analyze the malicious executable files. As lab suggests, we will use Detect It Easy (DIE), a powerful file analyzer that can identify file types, compilers, packers, and other characteristics of executable files.
<img width="1796" height="716" alt="image" src="https://github.com/user-attachments/assets/fdd79562-f934-4c75-8569-5e8881b53b84" />
Answer: requestFile
## Q15: One of the IP's the malware contacted starts with 17. Provide the full IP.  
<img width="1227" height="30" alt="image" src="https://github.com/user-attachments/assets/257c992b-1b71-47cb-abf3-168cab9fe853" />
Answer: 174.129.57.253

## Q16: How many files did the malware request from external servers?  
<img width="925" height="207" alt="image" src="https://github.com/user-attachments/assets/2397bd23-a302-4bd5-ab5a-0ac188e833e4" />
Answer: 9
## Q17: What are the commands that the malware was receiving from attacker servers? Format: comma-separated in alphabetical order.
<img width="387" height="389" alt="image" src="https://github.com/user-attachments/assets/1dfb6981-a0cc-4378-accf-bf6498183284" />
<img width="566" height="628" alt="image" src="https://github.com/user-attachments/assets/01041009-315e-40b0-8a50-74c8c5b22f89" />

Answer: NOP, RUN

<img width="550" height="159" alt="image" src="https://github.com/user-attachments/assets/7aac0b0e-34c1-4493-bd8b-241d0e7bfe19" />
