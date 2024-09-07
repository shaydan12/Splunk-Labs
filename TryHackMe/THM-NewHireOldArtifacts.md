&nbsp;

# A Web Browser Password Viewer executed on the infected machine. What is the name of the binary? Enter the full path.

Filtering by Image where a command was associated, I found a suspicious file in the "C:\\Users\\FINANC~1\\AppData\\Local\\Temp\\" directory

![image](https://github.com/user-attachments/assets/00f58417-8a8f-4daf-8779-771b81ad5a88)


<span style="color: #2dc26b;">**Answer: C:\\Users\\FINANC~1\\AppData\\Local\\Temp\\11111.exe**</span>

# What is listed as the company name?

Filter by the name of the image, and look into the "Company" field

![image](https://github.com/user-attachments/assets/1623c84f-0295-4230-b9c4-daa1ec26d6e7)


<span style="color: #2dc26b;">**Answer: NirSoft**</span>

# Another suspicious binary running from the same folder was executed on the workstation. What was the name of the binary? What is listed as its original filename? (format: file.xyz,file.xyz)

Looking into the temp folder, I found a suspicious binary: IonicLarge.exe

![image](https://github.com/user-attachments/assets/8634d8dc-761a-4980-a8f6-bfad1a890676)


&nbsp;

OriginalFileName isn't available by default, so I added it:

![image](https://github.com/user-attachments/assets/12815b60-8345-48be-9053-55c8a6878285)


&nbsp;

![image](https://github.com/user-attachments/assets/a71cf958-7011-4e06-ba36-baf3b56715eb)


&nbsp;

![image](https://github.com/user-attachments/assets/2e5bd039-a5f3-4e7f-82b8-52ef6ef33b89)


&nbsp;

&nbsp;

&nbsp;

# The binary from the previous question made two outbound connections to a malicious IP address. What was the IP address? Enter the answer in a defang format.

Filter for events with the binary from the previous question: `source="WinEventLog:Microsoft-Windows-Sysmon/Operational" Image="C:\\Users\\Finance01\\AppData\\Local\\Temp\\IonicLarge.exe"`

&nbsp;

In the DestinationIp field, it is shown that two outbound connections were made to 2\[.\]56\[.\]59\[.\]42

![image](https://github.com/user-attachments/assets/ffbbf272-814b-491b-8bed-dedb06807f0c)


&nbsp;

Confirmed that the ip address is malicious on [VirusTotal]

![image](https://github.com/user-attachments/assets/e7aca290-f35f-4e86-bf6a-962a2eed5c07)



<span style="color: #2dc26b;">**Answer: 2\[.\]56\[.\]59\[.\]42**</span>

# The same binary made some change to a registry key. What was the key path?

The sysmon Event code for Registry setting is 13.

Filter:

`source="WinEventLog:Microsoft-Windows-Sysmon/Operational" Image="C:\\Users\\Finance01\\AppData\\Local\\Temp\\IonicLarge.exe" EventCode=13`  
`| table TargetObject`

![image](https://github.com/user-attachments/assets/c5a45de9-d8a1-4c93-85c9-58def4b73d80)


Some registries were set in "HKLM\\SOFTWARE\\Policies\\Microsoft\\Windows Defender" in order to evade antivirus

<span style="color: #2dc26b;">**Answer: HKLM\\SOFTWARE\\Policies\\Microsoft\\Windows Defender**</span>

&nbsp;

# Some processes were killed and the associated binaries were deleted. What were the names of the two binaries? (format: file.xyz,file.xyz)

"taskkill /im" can be used to kill a process

&nbsp;

![image](https://github.com/user-attachments/assets/e07a5b42-b1c1-48db-a78f-b4c115f3c5ba)


<span style="color: #2dc26b;">**Answer: WvmIOrcfsuILdX6SNwIRmGOJ.exe,phcIAmLJMAIMSa9j9MpgJo1m.exe**</span>

&nbsp;

# The attacker ran several commands within a PowerShell session to change the behaviour of Windows Defender. What was the last command executed in the series of similar commands?

Filter by events where the Image is powershell and look at any commands that were executed:

![image](https://github.com/user-attachments/assets/482b93bb-9cfd-4aa3-9840-466961851feb)


WMIC is used here to change the behavior of Windows Defender

The last event shown is the answer

<span style="color: #2dc26b;">**Answer: powershell WMIC /NAMESPACE:\\\\root\\Microsoft\\Windows\\Defender PATH MSFT_MpPreference call Add ThreatIDDefaultAction_Ids=2147737394 ThreatIDDefaultAction_Actions=6 Force=True**</span>

# Based on the previous answer, what were the four IDs set by the attacker? Enter the answer in order of execution. (format: 1st,2nd,3rd,4th)

&nbsp;

The answers are shown in the screenshot in the previous question.

<span style="color: #2dc26b;">**Answer: 2147735503,2147737010,2147737007,2147737394**</span>

&nbsp;

# Another malicious binary was executed on the infected workstation from another AppData location. What was the full path to the binary?

Filter for events with binaries located in the AppData folder, while also looking at what tasks they performed:

`source="WinEventLog:Microsoft-Windows-Sysmon/Operational" Image="*\\AppData\\*.exe"`  
`| table Image, TaskCategory`  
`| dedup Image, TaskCategory`

![image](https://github.com/user-attachments/assets/29a802d8-3570-4266-b910-f0598fc55a44)

&nbsp;"C:\\Users\\Finance01\\AppData\\Roaming\\EasyCalc\\EasyCalc.exe" makes registry edits and network connections which is unusual for a calculator app

**Answer: C:\\Users\\Finance01\\AppData\\Roaming\\EasyCalc\\EasyCalc.exe**

# What were the DLLs that were loaded from the binary from the previous question? Enter the answers in alphabetical order. (format: file1.dll,file2.dll,file3.dll)

Filter for DLLs that the binary loaded that are not signed.

![image](https://github.com/user-attachments/assets/9f0659df-2fc4-4a0d-bbee-28c54d835ca3)

<span style="color: #2dc26b;">**Answer: ffmpeg.dll,nw.dll,nw_elf.dll**</span>
