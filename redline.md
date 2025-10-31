<img width="1357" height="271" alt="image" src="https://github.com/user-attachments/assets/30a185e4-aae0-42ac-8e40-0c202e9ef5ac" />

<img width="1081" height="294" alt="image" src="https://github.com/user-attachments/assets/bbb64f7a-50c5-4277-814c-32c534737b55" />

## Q1. What is the name of the suspicious process?
Use pstree command.<img width="624" height="50" alt="image" src="https://github.com/user-attachments/assets/425701ab-bebe-40a2-b1cd-873ae699e14f" />

Answer: oneetx.exe
## Q2. What is the child process name of the suspicious process?
Answer: rundll32.exe
<img width="588" height="78" alt="image" src="https://github.com/user-attachments/assets/68a73ef4-5412-4175-987c-b31828f12ff4" />

## Q3. What is the memory protection applied to the suspicious process memory region?
Answer: PAGE_EXECUTE_READWRITE
Use malfind plugin.
<img width="1725" height="52" alt="image" src="https://github.com/user-attachments/assets/f7f1db42-c406-4c50-9eda-e4f32a01f096" />


## Q4. What is the name of the process responsible for the VPN connection?
To identify network-related processes, lets go with windows.pslist 
<img width="259" height="72" alt="image" src="https://github.com/user-attachments/assets/5a8c5b77-8087-4af2-8e6e-cc0e87b70755" />
<img width="613" height="116" alt="image" src="https://github.com/user-attachments/assets/80d7bcab-307a-49a5-9c89-b1736054094c" />
Lets see the pid of this process.
<img width="587" height="190" alt="image" src="https://github.com/user-attachments/assets/c9b26223-4872-4ce8-bbb4-7ba7997bf8f7" />
Answer: outline.exe
## Q5. What is the attacker's IP address?
Lets examine the network connections with windows.netscan
<img width="879" height="29" alt="image" src="https://github.com/user-attachments/assets/cbd25a03-823c-4bde-8517-1ad38b7e3490" />
Answer: 77.91.124.20
## Q6. What is the full URL of the PHP file that the attacker visited?
In this case, i used *strings MemoryDump.mem | grep 77.91.124.20*. to grep entries having the IP of the attacker in them
<img width="676" height="30" alt="image" src="https://github.com/user-attachments/assets/76e5095b-7ee2-45e6-8083-51ecfb98d45c" />

Answer: http://77.91.124.20/store/games/index.php
## Q7. What is the full path of the malicious executable?
As the first one, use pstree
<img width="516" height="53" alt="image" src="https://github.com/user-attachments/assets/6e1e36c6-0f37-432b-b43d-cffe00052f42" />

Answer: C:\Users\Tammam\AppData\Local\Temp\c3912af058\oneetx.exe
<img width="518" height="144" alt="image" src="https://github.com/user-attachments/assets/d23c3eb7-b2aa-4d95-8a9f-e9d0e5096b51" />
