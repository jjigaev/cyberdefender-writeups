<img width="1357" height="268" alt="image" src="https://github.com/user-attachments/assets/b85c248d-3760-4f81-8ea8-fb902d2e1a30" />
<img width="1056" height="267" alt="image" src="https://github.com/user-attachments/assets/bfe9449a-85cd-4171-8c8d-3e7013dfede9" />

## Q1 Based on the accounts of the witnesses and individuals close to the victim, it has become clear that the victim was interested in trading. This has led him to invest all of his money and acquire debt. Can you identify the SHA256 of the trading application the victim primarily used on his phone?  
In this lab we will use ALEAPP. It is forensic tool for Android Logs Events And Protobuf Parser. After installing, i used GUI version of programm.
<img width="1102" height="543" alt="image" src="https://github.com/user-attachments/assets/430b91a6-f0a8-4323-8893-9801792aa557" />
We will need to specify the path to dump file, output path, and all modules that we need. Output file will be shown in HTML form.
We see several sections for any type of data, it is formatted and comfortable for investigation. 
<img width="267" height="768" alt="image" src="https://github.com/user-attachments/assets/3775f340-3972-4bd7-a102-0eb6b52faaf6" />


All installed apps stored in specific section, look for this section and we will find all installed apps and their hashes.
<img width="1223" height="548" alt="image" src="https://github.com/user-attachments/assets/7e9e17f3-9653-4401-b19f-cad8d7d95f67" />
Answer: 4f168a772350f283a1c49e78c1548d7c2c6c05106d8b9feb825fdc3466e9df3c

## Q2 According to the testimony of the victim's best friend, he said, "While we were together, my friend got several calls he avoided. He said he owed the caller a lot of money but couldn't repay now". How much does the victim owe this person?  
Look for all messages that victim phone has, we will need to look for all possible services. On the calls section, we see several missed calls. <img width="469" height="396" alt="image" src="https://github.com/user-attachments/assets/b9c43add-d635-4e78-b8e6-54fb3a0a3899" />
Several missed calls from **+201172137258**. The contacts name is Shady Wahab.
Knowing this, now we can narrow down the search in the SMS messages section.
<img width="447" height="221" alt="image" src="https://github.com/user-attachments/assets/081dc6bf-c237-4984-8575-be59cb022ba9" />

Answer: 250000

## Q3 What is the name of the person to whom the victim owes money?  
From previous question.
Answer: Shady Wahab

## Q4 Based on the statement from the victim's family, they said that on September 20, 2023, he departed from his residence without informing anyone of his destination. Where was the victim located at that moment?  
I looked for "Recent_Activity_0" tab. We can look for any clues, in this case the victim was using Google Maps application on this date.
<img width="488" height="370" alt="image" src="https://github.com/user-attachments/assets/0e1ea8e2-00ea-459f-a1e7-494000f612fb" />

Answer: The Nile Ritz-Carlton

## Q5 The detective continued his investigation by questioning the hotel lobby. She informed him that the victim had reserved the room for 10 days and had a flight scheduled thereafter. The investigator believes that the victim may have stored his ticket information on his phone. Look for where the victim intended to travel.  
I checked the discord chats.
<img width="236" height="290" alt="image" src="https://github.com/user-attachments/assets/e0da476e-ac63-4c2f-ba36-f4a43ec352c9" />
It is from the user rob1inson, Mob Museum is location in the Las Vegas, which can be easily googled.
Answer: Las Vegas

## Q6 After examining the victim's Discord conversations, we discovered he had arranged to meet a friend at a specific location. Can you determine where this meeting was supposed to occur?  
From the previous question.
Answer: The Mob Museum

Done!
<img width="580" height="140" alt="image" src="https://github.com/user-attachments/assets/8e1b0f59-cc86-412f-ac8f-21714384ddd2" />
