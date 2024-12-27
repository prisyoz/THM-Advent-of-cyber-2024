WebSockets let your browser and the server keep a constant line of communication open. It’s like keeping the phone line open so you can chat whenever you need to. Once that connection is set up, the client and server can talk back and forth without all the extra requests.

- Great for live chat apps, real-time games or any live data feed with constant updates
- After quick handshake to get things started, both sides can send messages whenever
- Less overhead, faster communication when you need data flowing in real-time

**Traditional HTTP Requests vs WebSocket**

- When using regular HTTP, browser sends a request to the server and server responds then close connection. If need new data, will have to make another request (eg chat app when browser would ‘ask’ server for updates every few seconds. - known as polling. works but inefficient)
- WebSockets - once connection established, remains open, allowing server to push updates whenever there’s something new. Faster and uses less resources
    
    

**Vulnerabilities**

Because of the need to stay open and active, can be taken advantage of

Common vulnerabilities:

- **Weak Authentication and Authorisation**
    - No built-in ways to handle user authentication or session validation
    - If controls not set up properly, attackers could slip in and get access to sensitive data or mess with the connection
- **Message Tampering**
    - Attackers could intercept and change messages if not encrypt.
        - Can inject harmful commands, perform illegal actions or tamper with sent data
- **Cross-Site WebSocket Hijacking (CSWSH)**
    - When attacker tricks a user’s browser into opening a WebSocket connection to another site.
    - If successful, attacker might be able to hijack that connect or access data meant for legitimate server
- **Denial of Service (DoS)**
    - Can be targeted by DoS attacks - flood server with tons of messages, potentially slowing down or crashing server

**WebSocket Message Manipulation**

- When an attacker intercepts and changes the messages sent between a web app and its server
    - Could bypass security checks, send unauthorised requests or alter key data like usernames, payment amounts, access levels
    - Changes take effect instantly without the user or server noticing immediately
- WebSocket connections don’t have the same security protections as traditional HTTP connections (eg End-to End Encryption)
