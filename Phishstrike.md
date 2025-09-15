# Scenario
As a cybersecurity analyst at an educational institution, you receive an alert about a phishing email targeting faculty members. 
The email appears to be from a trusted contact and claims a $625,000 purchase, providing a link to download an invoice.
Your task is to investigate the email using Threat Intel tools. Analyze the email headers and inspect the link for malicious content. Identify any Indicators of Compromise (IOCs) and document your findings to prevent potential fraud and educate faculty on phishing recognition.

## Q1. Identifying the sender's IP address with specific SPF and DKIM values helps trace the source of the phishing email. What is the sender's IP address that has an SPF value of softfail and a DKIM value of fail?
An SPF record is simply a method for authenticating the sender of an email. I used MXtoolbox <img width="854" height="63" alt="image" src="https://github.com/user-attachments/assets/8e31089e-69e4-47b4-b07c-0caefcbb6698" />
18.208.22.104

## Q2. Understanding the return path of an email is essential for tracing its origin. What is the return path specified in this email?
<img width="722" height="270" alt="image" src="https://github.com/user-attachments/assets/321349d9-76a0-4b8b-a71e-f2390c3472ab" />
erikajohana.lopez@uptc.edu.co

## Q3. Identifying the source of malware is critical for effective threat mitigation and response. What is the IP address of the server hosting the malicious file related to malware distribution?
We can hover over the hyperlink where we would click to "download the document and confirm the payment order", obtaining the following address
107.175.247.199

## Q4. Identifying malware that exploits system resources for cryptocurrency mining is critical for prioritizing threat mitigation efforts. The malicious URL can deliver several malware types. Which malware family is responsible for cryptocurrency mining?
I used URLhaus, which gives a URL history 
<img width="1573" height="140" alt="image" src="https://github.com/user-attachments/assets/e9917e79-75e1-4330-af4d-a1ce122559e5" />
CoinMiner

## Q5. Identifying the specific URLs malware requests is key to disrupting its communication channels and reducing its impact. Based on the previous analysis of the cryptocurrency malware sample, what does this malware request the URL?
<img width="1578" height="692" alt="image" src="https://github.com/user-attachments/assets/a9f35e86-54bb-4c8b-97cf-5096b5e30c83" />
I got the hash. then i looked for Virustotal. <img width="1916" height="748" alt="image" src="https://github.com/user-attachments/assets/ee434f38-dd3e-4cc6-a7ea-1906f4e7d618" />
<img width="1047" height="287" alt="image" src="https://github.com/user-attachments/assets/09726c7b-61f2-4bcf-ab66-0823ad68cbb5" />
http://ripley.studio/loader/uploads/Qanjttrbv.jpeg

## Q6. Understanding the registry entries added to the auto-run key by malware is crucial for identifying its persistence mechanisms. Based on the BitRAT malware sample analysis, what is the executable's name in the first value added to the registry auto-run key?
This one choose BitRat Signature and copy the SHA256 and look in VirusTotal for Registry behaviour.
<img width="799" height="84" alt="image" src="https://github.com/user-attachments/assets/2f63b09d-bfcc-44d4-83cd-989c14942d47" />
Jzwvix.exe

## Q7. Identifying the SHA-256 hash of files downloaded from a malicious URL is essential for tracking and analyzing malware activity. Based on the BitRAT analysis, what is the SHA-256 hash of the file previously downloaded and added to the autorun keys?
Check behaviour tab, then search for Jzwvix.exe under dropped files to get the hash. 
<img width="1121" height="106" alt="image" src="https://github.com/user-attachments/assets/f9562c3b-4a12-420c-8701-c611007a2444" />
BF7628695C2DF7A3020034A065397592A1F8850E59F9A448B555BC1C8C639539

## Q8. Analyzing the HTTP requests made by malware helps in identifying its communication patterns. What is the URL in the HTTP request used by the loader to retrieve the BitRAT malware?
<img width="953" height="24" alt="image" src="https://github.com/user-attachments/assets/8305d371-e242-4a8e-83ec-b19f3b1a4460" />
http://107.175.247.199/LOADER/SERVER.EXE

## Q9. Introducing a delay in malware execution can help evade detection mechanisms. What is the delay (in seconds) caused by the PowerShell command according to the BitRAT analysis?
<img width="2144" height="220" alt="image" src="https://github.com/user-attachments/assets/cc3a4868-3ea4-4f37-9a83-41233ec64586" />
<img width="835" height="711" alt="image" src="https://github.com/user-attachments/assets/551bbf2f-cca4-444e-a959-5d07bf732c40" />
50

## Q10. Tracking the command and control (C2) domains used by malware is essential for detecting and blocking malicious activities. What is the C2 domain used by the BitRAT malware?
<img width="1512" height="594" alt="image" src="https://github.com/user-attachments/assets/818ccc85-64fe-4d45-b037-6a66d71d59d2" />
gh9st.mywire.org

## Q11. Understanding how malware exfiltrates data is essential for detecting and preventing data breaches. According to the AsyncRAT analysis, what is the Telegram Bot ID used by this malware?
<img width="1526" height="30" alt="image" src="https://github.com/user-attachments/assets/b9e38ac3-c10e-44af-8817-053851da43eb" />
I got the AsyncRAT hash from urlhaus, searched on Virus Total, then checked for triage reports using the hash obtained. 
answer is bot5610920260
