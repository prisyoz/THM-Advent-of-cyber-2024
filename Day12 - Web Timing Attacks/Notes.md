Conventional web applications are relatively easy to understand, identify and exploit. If there is an issue in the code of the web application, we can force the web application to perform an unintended action by sending specific inputs. These are easy to understand because there is usually a direct relationship between the input and output. Bad output when send bad data, indicating a vulnerability.

What if vulnerabilities can be found using only good data? what if it isn’t about the data but how it is sent? - Web timing and race condition attacks

Web timing attack means we glean information from a web application by reviewing how long it takes to process our request. By making tiny changes in what we send or how we send it and observing the response time, we can access information we are not authorised to have.

Race conditions are a subset of web timing attacks that are even more special. With a race condition attack, we are no longer simply looking to gain access to information but can cause the web application to perform unintended actions on our behalf. 

Web timing vulnerabilities can be subtle. Based on research, reponse time differences ranging from 1300ms and 5ns have been used to stage attacks. Can be hard to detect and often requre wide range of testing techniques. With increase in adoption of HTTP/2, they have become a bit easier to find and exploit.

**Rise of HTTP/2**

- Created as a major update for HTTP, the protocol used for web applications.
- Steady increase in the adoption of HTTP/2
    - Faster and better for web performance
    - Several features that elevate the limitations of HTTP/1.1
- If implemented incorrectly, some of the new features can be exploited by using new techniques
- Key difference in web timing attacks between HTTP/1.1 and HTTP/2
    - HTTP/2 supports a feature called single-packet multi-requests
    - Network latency - amount of time it takes for the request to reach web browser, makes it difficult to identify web issues.
        - Hard to know the time difference is because of web timing vulnerability or just a network latency difference
    - Can stack multiple requests in the same TCP packet, eliminating network latency from the equation
    - Time differences can be attributed to different processing times for the requests
    - Eliminating network latency, only server latency remains, making it significantly easier to detect timing issues and exploit them

**Typical timing attacks**

Two main categories

- Information Disclosures
    
    Leveraging the differences in response delays, a threat actor can uncover information they should not have access to. Eg Timing differences can be used to enumerate the usernames of an application, making it easier to stage a password-guessing attack and gain access to accounts
    
- Race Conditions
    
    Similar to business logic flaws in that a threat actor can cause the application to perform unintended actions. However, the root cause is how the web application processes requests, making it possible to cause the race conditions. Eg By sending the same coupon request several times simultaneously, it might be possible to apply it more than once.
