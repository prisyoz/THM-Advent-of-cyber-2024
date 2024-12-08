**Monitoring in an AWS Environment**

Amazon Web Services (AWS) is a comprehensive cloud computing platform offered by Amazon., providing a wide range of services such as computing power, storage, databases, networking, analytics etc delivered over the internet. 

Instead of running workloads on physical machines on-premises, they run on virtualised instances in the cloud, which are called EC2 (Amazon Elastic Compute Cloud).

**CloudWatch**

AWS CloudWatch is a monitoring and observability platform that gives greater insight into AWS environment by monitoring applications at multiple levels. 

- Functionality - Monitoring of system and application metrics, Configuration of alarms on those metrics

Running an application in a cloud environment can mean leveraging a lot of different services (eg a service running the application, a service running functions triggered by that application, a service running the application backend etc), which will translate to logs being generated from a lot of different sources. 

CloudWatch logs makes it easy for users to access, monitor and store the logs from all these various sources. CloudWatch agent must be installed for application and system metrics to be captured.

- Key feature - Ability to query application logs using filter patterns
    - **Log events** - A single log entry recording an application “event” , timestamped and packaged with log messages and metadata
    - **Log Streams** - A collection of log events from a single source
    - **Log Groups** - A collection of log streams. Log streams are collected into a log group when logically it makes sense, eg same service running across multiple hosts

**CloudTrail**

Capture and record actions taken. Actions can be those taken by a user, a role (granted to a user giving them certain permissions) or an AWS service and are recorded as events in AWS CloudTrail. These actions could also be interactions with any number of AWS services (eg S3 - Amazon Simple Storage Service used for object storage, IAM - Identity and Access management service used to secure access to the AWS environment with the creation of identities and assigning of access permissions to those identities) will have actions taken within their service recorded

- Features
    - **Always on** - CloudTrail is enabled by default for all users
    - **JSON-formatted** - All event types captured by CloudTrail will be in the CloudTrail JSON format
    - **Event History** - When users access CloudTrail, they will see an option “Event History”, event history is a record of the actions that have taken place in the last 90 days. These records are queryable and can be filtered on attributes such as “resouce” type.
    - **Trails** - Event history can be thought of as the default “trail”, included out of the box. Users can define custom trails to capture specific actions, which is useful if you have bespoke monitoring scenarios you want to capture and store **beyond the 90-day event history rntention period**
    - **Deliverable** - Can be used as a single access point for logs generated from various sources. CloudTrail has an optional feature enabling CloudTrail logs to be delivered to CloudWatch

**JQ**

JQ is a lightweight and flexible command line processor that can be used on JSON (similar to line tools like sed, awk, grep).

```json
[

{ "book_title": "Wares Wally", "genre": "children", "page_count": 20 },

{ "book_title": "Charlottes Web Crawler", "genre": "young_ware", "page_count": 120 },

{ "book_title": "Charlie and the 8 Bit Factory", "genre": "young_ware", "page_count": 108 },

{ "book_title": "The Princess and the Pcap", "genre": "children", "page_count": 48 },

{ "book_title": "The Lion, the Glitch and the Wardrobe", "genre": "young_ware", "page_count": 218 }

]
```

**`jq '.[]' book_list.json`**

`.` - accessing current input. 

`[]` - access the array of values stored in our JSON (with the []), making the filter a `.[]`

Results

```json
{
  "book_title": "Wares Wally",
  "genre": "children",
  "page_count": 20
}
{
  "book_title": "Charlottes Web Crawler",
  "genre": "young_ware",
  "page_count": 120
}
{
  "book_title": "Charlie and the 8 Bit Factory",
  "genre": "young_ware",
  "page_count": 108
}
{
  "book_title": "The Princess and the Pcap",
  "genre": "children",
  "page_count": 48
}
{
  "book_title": "The Lion, the Glitch and the Wardrobe",
  "genre": "young_ware",
  "page_count": 218
}
```

**`'.[] | .book_title' book_list.json`** - to view only the book title

```json
"Wares Wally"
"Charlottes Web Crawler"
"Charlie and the 8 Bit Factory"
"The Princess and the Pcap"
"The Lion, the Glitch and the Wardrobe"
```

**S3 log entry example**

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

| **Field** | **Description** |
| --- | --- |
| userIdentity | Details of the user account that acted on an object. |
| eventTime | When did the action occur? |
| eventType | What type of event occurred? (e.g., AwsApiCall or AwsConsoleSignIn, AwsServiceEvent) |
| eventSource | From what service was the event logged? |
| eventName | What specific action occurred? (e.g., ListObjects, GetBucketObject) |
| sourceIPAddress | From what IP did the action happen? |
| userAgent | What user agent was used to perform the action? (e.g., Firefox, AWS CLI) |
| requestParameters | What parameters were involved in the action? (e.g., BucketName) |
