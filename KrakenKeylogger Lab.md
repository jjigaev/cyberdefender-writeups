<img width="1388" height="352" alt="image" src="https://github.com/user-attachments/assets/880cbc05-d41f-4afa-baba-81c75be77c5c" />
<img width="1107" height="374" alt="image" src="https://github.com/user-attachments/assets/4b317fb9-34f3-4bfd-b438-f06ade5773b9" />

## Q1 What is the the web messaging app the employee used to talk to the attacker?
In this assignment, we will use DB Browser for SQLite disk image. 
<img width="1131" height="801" alt="image" src="https://github.com/user-attachments/assets/4ba27e1d-8c70-4182-8947-950e07eeb8e3" />
Go to notification table and examine the rows in this table.
<img width="1063" height="634" alt="image" src="https://github.com/user-attachments/assets/7d842e76-3eed-4d37-b9b7-647c718f27f0" />
<img width="162" height="32" alt="image" src="https://github.com/user-attachments/assets/ce344c40-34e0-41eb-bbe0-1206721e800f" />
Answer: Telegram

## Q2 What is the password for the protected ZIP file sent by the attacker to the employee?
At the same log
<img width="94" height="26" alt="image" src="https://github.com/user-attachments/assets/25d01e7f-b330-4b08-851a-a07eaf05d23d" />
You can check the password to check search for .zip files.
Answer: @1122d

## Q3 What domain did the attacker use to download the second stage of the malware?
In the file we extracted this is .ink file we can check it
<img width="797" height="44" alt="image" src="https://github.com/user-attachments/assets/621db618-3e4e-4a09-89f2-48062b44bc28" />
I used Aptopsy, extracting the file using acquired password
<img width="1618" height="628" alt="image" src="https://github.com/user-attachments/assets/57851c7e-548e-46e9-a527-51a5d41a578a" />
We see a PowerShell script in the output with obfuscated URL.
Search by google
<img width="1032" height="464" alt="image" src="https://github.com/user-attachments/assets/e516744e-6258-46e0-848c-ddaf5d58504a" />
<img width="1209" height="332" alt="image" src="https://github.com/user-attachments/assets/ebd3b92f-1201-4c31-a4b5-905b5a65ef65" />
<img width="792" height="97" alt="image" src="https://github.com/user-attachments/assets/430823cd-73c0-4f5f-b33f-6b1e6329477a" />
Answer: masherofmasters.cyou

## Q4 What is the name of the command that the attacker injected using one of the installed LOLAPPS on the machine to achieve persistence?
Looking at the LOLAPPS compendium and installed applications in OMEN user profile we see the GREENSHOT application. <img width="1415" height="567" alt="image" src="https://github.com/user-attachments/assets/a7ee9dd8-e3f7-4bbf-831f-9668263608be" />
<img width="1144" height="819" alt="image" src="https://github.com/user-attachments/assets/45995540-ab3e-4d82-b9dc-e7d3b358e898" />
<img width="868" height="501" alt="image" src="https://github.com/user-attachments/assets/9836e1e2-3c7d-4da7-b577-115b79ef3b58" />
<img width="1053" height="211" alt="image" src="https://github.com/user-attachments/assets/924943da-572f-48ba-baa8-32a3c703207a" />
Answer: jlhgfjhdflghjhuhuh

## Q5 What is the complete path of the malicious file that the attacker used to achieve persistence?
This one in the same Greenshot.ini file. <img width="1216" height="118" alt="image" src="https://github.com/user-attachments/assets/8009867b-ab33-4946-9c48-3be5800b0a64" />
Answer: C:\Users\OMEN\AppData\Local\Temp\templet.lnk

## Q6 What is the name of the application the attacker utilized for data exfiltration?
<img width="872" height="322" alt="image" src="https://github.com/user-attachments/assets/7641e61c-4286-454c-834c-441ced63d6a0" />
Answer: AnyDesk

## Q7 What is the IP address of the attacker?
Now we know which application was used for data exfiltration and we saw ad.trace file which we open and see the IP address.
<img width="608" height="279" alt="image" src="https://github.com/user-attachments/assets/29446a70-7a50-4fb4-86fb-8f7bec82dfa6" />

Answer: 77.232.122.31

<img width="604" height="154" alt="image" src="https://github.com/user-attachments/assets/f4fa8a5e-91da-43b8-b7a0-236f07b238fb" />
