The responsible ware for the Care4Wares charity drive gave us the following info regarding this incident:

*We sent out a link on the 28th of November to everyone in our network that points to a flyer with the details of our charity. The details include the account number to receive donations. We received many donations the first day after sending out the link, but there were none from the second day on. I talked to multiple people who claimed to have donated a respectable sum. One showed his transaction, and I noticed the account number was wrong. I checked the link, and it was still the same. I opened the link, and the digital flyer was the same except for the account number.*

McSkidy recalls putting the digital flyer, **wareville-bank-account-qr.png**, in an Amazon AWS S3 bucket named **wareville-care4wares**. Let’s assist McSkidy and start by finding out more about that link. Before that, let’s first review the information that we currently have to start the investigation:

- The day after the link was sent out, several donations were received.
- Since the second day after sending the link, no more donations have been received.
- A donator has shown proof of his transaction. It was made 3 days after he received the link. The account number in the transaction was not correct.
- McSkidy put the digital flyer in the AWS S3 object named **wareville-bank-account-qr.png** under the bucket **wareville-care4wares**.
- The link has not been altered.

**S3 Log Entry**

```json
{
  "eventVersion": "1.10",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAXRMKYT5O5Y2GLD4ZG",
    "arn": "arn:aws:iam::518371450717:user/wareville_collector",
    "accountId": "518371450717",
    "accessKeyId": "AKIAXRMKYT5OZCZPGNZ7",
    "userName": "wareville_collector"
  },
  "eventTime": "2024-10-21T22:13:24Z",
  "eventSource": "s3.amazonaws.com",
  "eventName": "ListObjects",
  "awsRegion": "ap-southeast-1",
  "sourceIPAddress": "34.247.218.56",
  "userAgent": "[aws-sdk-go/0.24.0 (go1.22.6; linux; amd64)]",
  "requestParameters": {
    "bucketName": "aoc-cloudtrail-wareville",
    "Host": "aoc-cloudtrail-wareville.s3.ap-southeast-1.amazonaws.com",
    "prefix": ""
  },
  "responseElements": null,
  "additionalEventData": {
    "SignatureVersion": "SigV4",
    "CipherSuite": "TLS_AES_128_GCM_SHA256",
    "bytesTransferredIn": 0,
    "AuthenticationMethod": "AuthHeader",
    "x-amz-id-2": "yqniVtqBrL0jNyGlvnYeR3BvJJPlXdgxvjAwwWhTt9dLMbhgZugkhlH8H21Oo5kNLiq8vg5vLoj3BNl9LPEAqN5iHpKpZ1hVynQi7qrIDk0=",
    "bytesTransferredOut": 236375
  },
  "requestID": "YKEKJP7QX32B4NZB",
  "eventID": "fd80529f-d0af-4f44-8034-743d8d92bdcf",
  "readOnly": true,
  "resources": [
    {
      "type": "AWS::S3::Object",
      "ARNPrefix": "arn:aws:s3:::aoc-cloudtrail-wareville/"
    },
    {
      "accountId": "518371450717",
      "type": "AWS::S3::Bucket",
      "ARN": "arn:aws:s3:::aoc-cloudtrail-wareville"
    }
  ],
  "eventType": "AwsApiCall",
  "managementEvent": false,
  "recipientAccountId": "518371450717",
  "eventCategory": "Data",
  "tlsDetails": {
    "tlsVersion": "TLSv1.3",
    "cipherSuite": "TLS_AES_128_GCM_SHA256",
    "clientProvidedHostHeader": "aoc-cloudtrail-wareville.s3.ap-southeast-1.amazonaws.com"
  }
}
```

- IAM user, **wareville_collector**, listed all objects (ListObjects event) of the S3 bucket named **aoc-cloudtrail-wareville**
- IP address originated **34.247.218.56**
- User agent indicates request was made using **AWS SDK tool for Go**
  
![07SS1](https://github.com/user-attachments/assets/a08837ac-c04f-41e3-bb34-1e546df0926a)

`cloudtrail_log.json`

![07SS2](https://github.com/user-attachments/assets/84cb6251-b96c-4bb8-9b50-bfc51b244ffc)

![07SS3](https://github.com/user-attachments/assets/e1e583f6-d456-4bda-a8a2-392d39f6da69)

**`jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares")' cloudtrail_log.json`**

| Command | Description |
| --- | --- |
| **`jq -r 'FILTER' cloudtrail_log.json`** | • The **-r** flag tells **jq** to output the results in RAW format instead of JSON. 
• Note that the **FILTER** section is enclosed with single quotes.
• The last part of the command accepts the input file, which is **cloudtrail_log.json**. |
| **`.Records[]`** | • Instructs **jq** to parse the events in the Records container element. The **Records** field is the top element in the JSON-formatted CloudTrail log. |
| **`| select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares")`** | • Uses the previous command's output, and filters it on the **eventSource** and **requestParameters.bucketName** keys.
• The value **s3.amazonaws.com** is used to filter events related to the Amazon AWS S3 service, and the value  **wareville-care4wares** is used to filter events related to the target S3 bucket. |

![07SS4](https://github.com/user-attachments/assets/160ae600-8b3f-49ea-82ac-99ab888d7111)

**`jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]' cloudtrail_log.json`**

| Command | Description |
| --- | --- |
| **`| [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"])'`** | • The piped filter uses the previous command's output and formats it to only include the defined keys, such as **.eventTime**, **.eventName**, and **.userIdentity.userName**.
• The defined keys are enclosed with square brackets (**`[]`**)  **to process and create an array with the specified fields from each record**.
• Note that the string **`// "N/A"`** is included purely for formatting reasons. This means that if the defined key does not have a value, it will display **N/A** instead. |

**`jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"],(.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t`**

| Command | Description |
| --- | --- |
| **`jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"], SELECT_FILTER | SPECIFIC FIELDS'`** | • The new command prepends a column header row and is defined using square brackets since it is an array that corresponds to the selected fields.
• Note that a comma is used before the select filter to combine with those of the select filter results we previously used. |
| **`| @tsv'`** | • Sets each array element, the output processed after the filters, as a line of tab-separated values. |
| **`| column -t -s $'\t'`** | • It takes the output of the **jq** command, now resulting in tab-separated values, and beautifies its result by processing all tabs and aligning the columns. |

![07SS5](https://github.com/user-attachments/assets/c1ca9f6a-b420-43a1-b1cc-adb7cf9521ee)

Now that we have crafted a JQ query that provides a well-refined output, let’s look at the results and observe the events. Based on the columns, we can answer the following questions to build our assumptions:

- How many log entries are related to the **wareville-care4wares** bucket?
- Which user initiated most of these log entries?
- Which actions did the user perform based on the **eventName** field?
- Were there any specific files edited?
- What is the timestamp of the log entries?
- What is the source IP related to these log entries?

5 logged events related to **wareville-care4wares** bucket, mostly related to user glitch. User glitch uploaded **wareville-bank-account-qr.png** on November 28 - which seems to coincide with the information received about no donations being made 2 days after the link was sent out

McSkidy knows there’s not user glitch in the system before, neither is there anyone in the city hall with that name. But knows there’s a hacker with the name who keeps to himself.

McSkidy wants to know what this anomalous user account has been used for, when it was created, and who created it. Enter the command below to see all the events related to the anomalous user. We can focus our analysis on the following questions:

- What event types are included in these log entries?
- What is the timestamp of these log entries?
- Which IPs are included in these log entries?
- What tool/OS was used in these log entries?

**`jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'`**

![07SS7](https://github.com/user-attachments/assets/e4c888c2-bbe9-4238-a27f-52f195925408)

Mostly targeted S3 bucket. 

Notable event - **ConsoleLogin** entry, which tells us that the account was used to access the AWS Management Console using a browser.

**`jq -r '["Event_Time", "Event_type", "Event_Name", "User_Name", "Source_IP", "User_Agent"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime,.eventType, .eventName, .userIdentity.userName //"N/A",.sourceIPAddress //"N/A", .userAgent //"N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'`**

![07SS8](https://github.com/user-attachments/assets/ae87e26e-2f0f-4b80-9f35-a8ca04b21afc)

| Command | Description |
| --- | --- |
| **`S3Console/0.4, aws-internal/3 aws-sdk-java/1.12.750 Linux/5.10.226-192.879.amzn2int.x86_64 OpenJDK_64-Bit_Server_VM/25.412-b09 java/1.8.0_412 vendor/Oracle_Corporation cfg/retry-mode/standard`** | • This is the userAgent string for the internal console used in AWS. It doesn’t provide much information. |
| **`Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/129.0.0.0 Safari/537.36`** | • This userAgent string provides us with 2 pieces of interesting information.
• The anomalous account uses a Google Chrome browser within a Mac OS system. |

**Who created the anomalous user account**

Filter for all IAM-related events using select filter `.eventSource == "iam.amazonaws.com"`

- What Event Names are included in the log entries?
- What user executed these events?
- What is this user’s IP?

**`jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.eventSource == "iam.amazonaws.com") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'`**

![07SS9](https://github.com/user-attachments/assets/30abe466-3aed-4d0a-a94d-6427fccda99e)

There’s a **CreateUser** action and consequently invoking the **AttachUserPolicy** action by user mcskidy. The source IP is **53.94.201.69,** which is the same IP the user glitch used.

More detailed look at the event related to **CreateUser** action

**`jq '.Records[] |select(.eventSource=="iam.amazonaws.com" and .eventName== "CreateUser")' cloudtrail_log.json`**

![07SS10](https://github.com/user-attachments/assets/e70c47aa-a9e7-49c4-bd76-ab71c4722df8)

**`jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "AttachUserPolicy")' cloudtrail_log.json`**

![07SS12](https://github.com/user-attachments/assets/388a1224-ace6-4029-bf7c-6c8552cab937)

The logs show that user mcskidy created user glitch account and assigned privileged access. 

But Mcskidy does not recognise the IP address involved in the events and does not use a Mac OS, only a Windows machine. 

**Closer detail on IP address and OS**

**`jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.sourceIPAddress=="53.94.201.69") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'`**

![07SS13](https://github.com/user-attachments/assets/055cfe68-50ae-4eb7-b42c-b840fcfb000c)

Three user accounts (mcskidy, glitch and mayor_malware) accessed  from the same IP address. 

Check each user to see if they always work from that IP

While gathering the information for each user, we can focus our investigation on the following questions:

- Which IP does each user typically use to log into AWS?
- Which OS and browser does each user usually use?
- Are there any similarities or explicit differences between the IP addresses and operating systems used?

**`jq -r '["Event_Time","Event_Source","Event_Name", "User_Name","User_Agent","Source_IP"],(.Records[] | select(.userIdentity.userName=="PLACEHOLDER") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A",.userAgent // "N/A",.sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'`**

![07SS14](https://github.com/user-attachments/assets/79504f73-2b57-45cf-8acf-99418797b2a0)

user: mcskidy

![07SS15](https://github.com/user-attachments/assets/77af9369-8d10-4b4c-a9cc-ada7f6eb2d2f)

user: glitch

![07SS16](https://github.com/user-attachments/assets/b5ab55b3-5d11-4052-b2e6-c07c283a4e4d)

user: mayor_malware

Conclusion: user glitch and mayor_malware typically log into AWS with the same IP address 53.94.201.69 and using Mac OS.

- The incident starts with an anomalous login with the user account **mcskidy** from IP **53.94.201.69**.
- Shortly after the login, an anomalous user account **glitch** was created.
- Then, the **glitch** user account was assigned administrator permissions.
- The **glitch** user account then accessed the S3 bucket named **wareville-care4wares** and replaced the **wareville-bank-account-qr.png** file with a new one. The IP address and User-Agent used to log into the **glitch, mcskidy**, and **mayor_malware** accounts were the same.
- the User-Agent string and Source IP of recurrent logins by the user account **mcskidy** are different.

**Evidence**

Wareville Bank cooperated and provided their database logs from their Amazon Relational Database Service (RDS), also mentioning that these are captured through their CloudWatch, which differs from the CloudTrail logs as they are not stored in JSON format.

**wareville_logs/rds.log**

![07SS17](https://github.com/user-attachments/assets/a85a263e-cb39-461f-960c-cd798c8b946a)


**`grep INSERT rds.log`**

![07SS18](https://github.com/user-attachments/assets/151b3c2a-d8a3-464b-8bef-2a27d01e6ca6)


INSERT queries from RDS log pertain to who received the donations made by the townspeople. 

![07SS19](https://github.com/user-attachments/assets/05e33197-0aca-4978-a227-c60cb03fee75)

Funds being directed to two recipients on Nov 28 2024. Care4wares Fund and Mayor_Malware.

**Conclusion**

| **Timestamp** | **Source** | **Event** |
| --- | --- | --- |
| 2024-11-28 15:22:18 | CloudWatch RDS logs (rds.log) | Last donation received by the Care4wares Fund. |
| 2024-11-28 15:22:39 | CloudTrail logs (cloudtrail_log.json) | Bank details update on S3 bucket. |
| 2024-11-28 15:23:02 | CloudWatch RDS logs (rds.log) | First donation received by Mayor Malware. |
