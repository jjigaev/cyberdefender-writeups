<img width="1325" height="298" alt="image" src="https://github.com/user-attachments/assets/d506e4dd-110d-42c5-af78-43df3409b55f" />
<img width="1120" height="314" alt="image" src="https://github.com/user-attachments/assets/536a8ff2-b59e-4d92-96a4-b0bbd752e329" />

## Q1 What time was the RAM image acquired according to the suspect system?  
After we install the memory dump, we will use the windows.info.Info plugin in Vol3. This plugin provides key information about dump, this also includes system time.

<img width="621" height="32" alt="image" src="https://github.com/user-attachments/assets/75c464c7-6765-4604-adf7-ffe7ab42821e" />
Answer: 2021-04-30 17:52  

## Q2 What is the SHA256 hash value of the RAM image?  
<img width="815" height="84" alt="image" src="https://github.com/user-attachments/assets/382594ca-b9a7-4638-87d4-b5463422f2ef" />
Use Get-FileHash command, write the path to the memory dump and specify the hash algorithm we need. 
Answer: 9db01b1e7b19a3b2113bfb65e860fffd7a1630bdf2b18613d206ebf2aa0ea172  

## Q3 What is the process ID of brave.exe?  
I got the PID by pslist.PsList plugin.
Answer: 4856  

## Q4 How many established network connections were there at the time of acquisition?  
<img width="1668" height="252" alt="image" src="https://github.com/user-attachments/assets/3ec8a02e-bb2f-40c8-945c-fbfa1d679df4" />
I used windows.netscan.NetScan plugin.
Answer: 10  

## Q5 Which domain name does Chrome have an established network connection with?  
<img width="674" height="42" alt="image" src="https://github.com/user-attachments/assets/0a3a9ecc-2224-4beb-a51f-09deeee6c8e7" />
We will use this information to narrow down the search.
Just use this IP address to web search.
If wee lookup this address, it shows it belongs to Proton AG, the domain name is protonmail.ch.
Answer: protonmail.ch  

## Q6 What is the MD5 hash value of the process executable for PID 6988?  
pslist.PsList and --dump argument for command. Then we get the hash by Get-FileHash.
<img width="217" height="37" alt="image" src="https://github.com/user-attachments/assets/b0893450-523a-42bd-a887-897626d7f6da" />
Answer: 0b493d8e26f03ccd2060e0be85f430af  

## Q7 Can you identify the word that begins at offset 0x45BE876 and is 6 bytes long?  
To work with offset, we need the hex editor that allows investigators to view and manipulate the raw binary data of a file
<img width="338" height="40" alt="image" src="https://github.com/user-attachments/assets/11bb3683-99b4-4bd1-ad96-c9af8f497922" />
Answer: hacker  

## Q8 What is the creation date and time of the parent process of powershell.exe?  
I ran the pstree.PsTree plugin on memory dump and found the parent process.
Answer: 2021-04-30 17:39  

## Q9 What is the full path and name of the last file opened in notepad?
<img width="809" height="34" alt="image" src="https://github.com/user-attachments/assets/496fdd11-703b-41f8-a5ca-f03c30064719" />
I used windows.cmdline.CmdLine plugin.
Answer: C:\Users\JOHNDO~1\AppData\Local\Temp\7zO4FB31F24\accountNum  

## Q10 How long did the suspect use Brave browser? (In Hours)  
We will extract registry keys data from memory dump. For this, we need to use windows.registry.userassist.
"The UserAssist key is a valuable resource in forensic investigations, as it tracks applications executed by the user through the Windows Explorer shell."
The output of every specific registry keys would always be large. So we usually need to narrow down our search, i used "grep -i Brave" command 
<img width="972" height="33" alt="image" src="https://github.com/user-attachments/assets/0816fd09-80f8-469a-b264-687c5cbd652a" />

Answer: 4

<img width="553" height="319" alt="image" src="https://github.com/user-attachments/assets/933e78b4-5f7e-4175-bb4e-0932927a5eca" />
