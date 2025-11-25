<img width="1254" height="272" alt="image" src="https://github.com/user-attachments/assets/f303ed81-c686-45db-b1bb-43ceb061f0e7" />

In this assignment, we are working with Windows System memory dump, that we need to analyze using Volatility tools. In this case, i am using Volatility3 because it has built-in OS plugins, so i dont
need to pick up OS system for the memory dump manually, so it is easier that Vol2. Although the site recommends volatilty2, i find myself using volatility2 less and less, I only use it if Volatility3 really fails at doing something.
## Q1 What is the process ID of the currently running malicious process?
pslist to gain overview windows.pslist.PsList.
<img width="381" height="113" alt="image" src="https://github.com/user-attachments/assets/3a50ce0e-b3e3-490e-a3dd-bb64a3c40741" />
This processes are very suspicious cause of the names, so lets examine pstrees of this. It shares the same parent PID as many other processes. We can filter again for pid 4824 using grep and find this
<img width="354" height="25" alt="image" src="https://github.com/user-attachments/assets/390443d4-1d80-47d4-b4dc-ead053fe0cb3" />
That is definitely suspiciuous. There is only 5 processes active with this PID. We check every one of them by dumping the executables via --dump.
<img width="702" height="172" alt="image" src="https://github.com/user-attachments/assets/e76ba9aa-e6cd-47cd-882b-b5d21dc2efbd" />

vol3 -f memdump.mem windows.pslist.PsList --pid 6268 --dump
vol3 -f memdump.mem windows.pslist.PsList --pid 3372 --dump
vol3 -f memdump.mem windows.pslist.PsList --pid 2200 --dump
vol3 -f memdump.mem windows.pslist.PsList --pid 3884 --dump
vol3 -f memdump.mem windows.pslist.PsList --pid 8560 --dump

The only returned result was pid 8560, it showed up in Virustotal.
Answer: 8560
## Q2 What is the md5 hash hidden in the malicious process memory?
To dump the actual memory and its pages, we need to use windows.memmap.Memmap. We will dump pid 8560, and use *strings* command to output the dump to .txt file format.
<img width="855" height="60" alt="image" src="https://github.com/user-attachments/assets/d8fc757c-4e35-4320-958a-8a3dd4a87909" />
It is base64 code, decode it in Cyberchef.
Answer: 3a19697f29095bc289a96e4504679680

## Q3 What is the process name of the malicious process parent?
Figured this out at the beginning
Answer: explorer.exe

## Q4 What is the MAC address of this machine's default gateway?
Since it is Windows system, we need to look in the Registry via windows.registry.hivelist.HiveList
It usually should be under /Network registry.
Answer: 00:50:56:FE:D8:07

## Q5 What is the name of the file that is hidden in the alternative data stream?
In this one, i had to look up *alternative data stream*.
Alternate Data Streams (ADS) are a common technique to hide files. You willl need to parse the MFT to locate files with ADS. To detect this with Volatility you have to use mftparser plugin
According to websites a way to detect an ADS is to filter on *$DATA ADS*
<img width="1189" height="139" alt="image" src="https://github.com/user-attachments/assets/e0caaa37-6b57-40a6-b3c9-fa334ea9b3dd" />
i used windows.filescan.FileScan since mftparser is available on Vol2. Using filescan with search for potential files ending with ‘:’ and dumping the output to dumpfile, then using grep we found.
Answer: yes.txt
## Q6 What is the full path of the browser cache created when the user visited "www.13cubed.com"?
To find the browser cache path for "www.13cubed.com," we can start by searching the memory dumps file listings. A common approach is to use the `filescan` plugin output and filter for the domain name:
```bash
cat filescan.txt | grep 13cubed
```
This search returned a path, but it appears to be corrupted or poorly decoded in the output, making it unreliable. 
While the path fragment suggests the username is `CTF` (indicating a path like `Users\CTF\...`), this alone is not enough and was incorrect for the challenge.

So the question was asking for specific directory path that was *created* when the user first visited "www.13cubed.com." This kind of file system metadata, including the creation of new folders, is definitively recorded in the **MFT (Master File Table)**. 
Therefore, we need to analyze the MFT entries. But vol3 do not work with this, so in the end we had to use Vol2 with its manual system OS plugins.
```bash
vol.py -f memdump.mem --profile=Win10x64_17134 mftparser > mft.txt #to gain info about profile, we need to use memdump.mem windows.info.Info in Vol3
cat mft.txt | grep -i 13cubed
```
<img width="1297" height="22" alt="image" src="https://github.com/user-attachments/assets/e447c8f5-89a7-4d4a-a084-692540ca1d16" />
Answer: C:\Users\CTF\AppData\Local\Packages\MICROS~1.MIC\AC\#!001\MICROS~1\Cache\AHF2COV9\13cubed[1].htm

 
<img width="563" height="170" alt="image" src="https://github.com/user-attachments/assets/655c1896-fa10-49a2-ab40-6a8cc26a3d53" />

