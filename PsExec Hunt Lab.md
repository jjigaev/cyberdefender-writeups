<img width="1281" height="195" alt="image" src="https://github.com/user-attachments/assets/231d84ed-e798-4acb-b122-2f6f53ea698b" />
# Scenario
An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. 
Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.

## Q1. To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?  
Look for IP addresses that have high traffic volume or frequent connections.
<img width="315" height="73" alt="image" src="https://github.com/user-attachments/assets/b163f22a-d41a-4a1b-b2ff-fc9374fae5ae" />
<img width="996" height="25" alt="image" src="https://github.com/user-attachments/assets/1198fe07-01e4-4fee-b914-3fb6facb77a0" />
As we know, attacker used SMB attack to gain access, which is often associated with file-sharing and network resource access on Windows networks.
Answer: 10.0.0.130
## Q2. To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?  
Lets check the SMB communication sequence in detail, we can trace the attacker's movement and interactions with target systems.<img width="374" height="75" alt="image" src="https://github.com/user-attachments/assets/26c32990-4d3c-43eb-8de4-680df6fe328a" />

Answer: Sales-PC
## Q3. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?  
Answer: ssales
<img width="1291" height="43" alt="image" src="https://github.com/user-attachments/assets/2f4b2907-f17f-469a-8690-b77ee871f300" />

## Q4. After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?  
Right after the attack, we see the attacker go to the Admin panel. <img width="1259" height="20" alt="image" src="https://github.com/user-attachments/assets/0d31b11b-91fb-4822-9be1-3e70dd36894a" />
Then, we see file uploading requests.
<img width="484" height="138" alt="image" src="https://github.com/user-attachments/assets/46b5f3b5-7cf4-4212-963b-73429cc535a4" />
Answer: psexesvc
## Q5. We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?  
Answer: ADMIN$
<img width="215" height="31" alt="image" src="https://github.com/user-attachments/assets/e040f47f-9074-4d6c-b8bd-0a4d9585446e" />
## Q6. We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?  
We analyze the SMB Tree Connect Requests made earlier in the captured traffic. We see the only correct answer in SMB sequence.
<img width="1298" height="61" alt="image" src="https://github.com/user-attachments/assets/c65ade0f-9943-4614-9dad-ea624d4c8527" />
Answer: IPC$
## Q7. Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?
After the 1st attack, there is a lot of attack requests. But in the end we the some change in packets.
<img width="1684" height="421" alt="image" src="https://github.com/user-attachments/assets/dc3a56dd-248d-4325-bc03-d1eca398ce5e" />
As we see, attacker tries another user for attack.
<img width="190" height="19" alt="image" src="https://github.com/user-attachments/assets/6b3ade6a-5578-4acd-b99e-501da2ce3d2a" />
We see in the late packets, that attacker uses this acc to eslacation. So we need to find smth to this user earlier.
<img width="512" height="28" alt="image" src="https://github.com/user-attachments/assets/8f7ca968-fece-449e-8a5e-75ebb9400ac4" />

<img width="1243" height="533" alt="image" src="https://github.com/user-attachments/assets/ce43c0bc-4d45-4c69-89fe-8a7b7908d3d3" />

Answer: Marketing-PC

<img width="573" height="178" alt="image" src="https://github.com/user-attachments/assets/d7678906-2ed2-4284-a84c-bf662d4b47ee" />
