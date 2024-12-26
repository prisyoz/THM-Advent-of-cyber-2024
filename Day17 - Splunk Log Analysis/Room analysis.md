Checking of web and CCTV

- *web_logs* - contains events related to web connections to and from the CCTV web server
- *cctv_logs* - contains information about CCTV application access logs

**Summary of CCTV feed**

- Contain successful and failed login attempts from various users
- A few suspicious failed login attempts
- Information about CCTV footage being watched and downloaded

`index=cctv_feed | stats count(Event) by UserName` 

![17SS01](https://github.com/user-attachments/assets/2a593840-5786-477b-98ca-4c379e9d78dc)

`index=cctv_feed | stats count by Event` 

![17SS02](https://github.com/user-attachments/assets/f4b2cbe7-9a70-4bdb-942d-3a73fe48db54)

`index=cctv_feed | rare Event` 

![17SS03](https://github.com/user-attachments/assets/29ceae06-1925-497d-ae40-5a0e66c31706)

A few attempts to delete recording and failed login attempts

`index=cctv_feed *failed* | table _time UserName Event Session_id` 

![17SS04](https://github.com/user-attachments/assets/22ed9224-2181-4727-84bd-27c32d261465)

4 usernames but same session ID

Narrow down results to see what events are associated with `Session_id` 

`index=cctv_feed *put_Session_id_here* | table _time UserName Event Session_id` 

![17SS05](https://github.com/user-attachments/assets/17bc512a-a85d-4c45-b325-618e05ee2555)

`index=cctv_feed *Delete*`

![17SS06](https://github.com/user-attachments/assets/7f2543ee-10fd-4e49-8a10-e6ba3a970e9f)

Match the cctv_logs against the web_logs

![17SS07](https://github.com/user-attachments/assets/25bf8ea9-e2e8-4098-91a8-6c07df31d520)

One IP address 10:11:105:33 associated with the suspicious session ID

A check into the client ip 10:11:105:33 shows 2 other sessions

![17SS08](https://github.com/user-attachments/assets/78d9c249-3793-4abb-a2d2-b3064e9c5872)

Hence, we will need to observe what kind of activities captured were associated with the IP and session IDs

`index=web_logs clientip="10.11.105.33" | table _time clientip status uri ur_path file` 

![17SS09](https://github.com/user-attachments/assets/381bcce3-e9a7-4e6d-b6b9-88e697b45ae8)

When session ID change, can see logout events `lsr1743nkskt3r722momvhjcs3`

`index=cctv_feed **lsr1743nkskt3r722momvhjcs3*`*

![17SS10](https://github.com/user-attachments/assets/b708e1e6-adca-4e21-8a5a-babe71f1b943)

WE are able to locate the username = mmalware

Summary of investigation

- Attacker bruteforce attempt on various accounts.
- There was a successful login after the failed attempts.
- Attacker watched some of the camera streams.
- Multiple camera streams were downloaded.
- Followed by the deletion of the CCTV footage.
- The web logs had an IP address associated with the attacker's session ID.
- We found two other session IDs associated with the IP address.
- We correlated back to the cctv_feed logs to find the traces of any evidence revolving around those session IDs, and found the name of the attacker.
