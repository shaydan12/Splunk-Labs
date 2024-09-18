&nbsp;

# Creating alerts for successful log-in attempts

```
EventCode=4624 source="WinEventLog:Security"| table Source_Network_Address, Workstation_Name, Account_Domain, Account_Name, ComputerName, _time
```

![image](https://github.com/user-attachments/assets/f9c871fe-fcb8-41da-898a-d8946b085d60)

Saving as alert:

![image](https://github.com/user-attachments/assets/0a583ef5-c4f2-4e6d-bbae-7e6d683ab559)

&nbsp;

&nbsp;

Login from RDP on remote machine:

![image](https://github.com/user-attachments/assets/7d750fc5-1446-4615-b3b3-969f45e0a88b)

&nbsp;

On the alert page:

![image](https://github.com/user-attachments/assets/d2c9927e-0731-4558-9251-6ea1788929d7)

&nbsp;

Viewing the results:

![image](https://github.com/user-attachments/assets/3f39d7d5-7116-4a60-9a4d-c333b0ebe743)
