**Extensible Markup Language (XML)**

XML is a commonly used method to transport and store data in a structured format that humans and machines can easily understand. Eg. Two computers need to communicate and share data and both need to agree on a common format for exchanging information. This agreement (format) is known as `XML`.

XML uses `tags` to label and organise information, just like folders in a digital filing cabinet.

```xml
<people>
   <name>Glitch</name>
   <address>Wareville</address>
   <email>glitch@wareville.com</email>
   <phone>111000</phone>
</people>
```

*<people> <name> <address>* are like folders in a filing cabinet, storing data about Glitch. The content inside the tags `Glitch` `Wareville` `111-000` represents actual data being stored. 

Key benefit is easily shareable and customisable, allowing for creation of own tags.

**Document Type Definition (DTD)**

DTD is a set of **rules** that defines the structure of an XML document. Just like a database scheme, it acts like a blueprint, telling you what elements (tags) and attributes are allowed in the XML file, like a guideline that ensures the XML document follows a specific structure.

For example, if we want to ensure that an XML document about **`people`** will always include a **`name`**, **`address`**, **`email`**, and **`phone number`**, we would define those rules through a DTD as shown below:

```xml
<!DOCTYPE people [
   <!ELEMENT people(name, address, email, phone)>
   <!ELEMENT name (#PCDATA)>
   <!ELEMENT address (#PCDATA)>
   <!ELEMENT email (#PCDATA)>
   <!ELEMENT phone (#PCDATA)>
]>
```

`<!ELEMENT>` defines the elements (tags) that are allowed, like name, address, email and phone whereas `$PCDATA` stands for parsed `people` data, meaning it will consist of just plain text.

**Entities**

So far, both computers have agreed on the format, structure of data nad type of data they will share. Entities in XML are placeholders that allow the insertion of large *chunks* of data or referencing internal or external files. They assist in making the XML file easy to manage, especially when the same data is repeated multiple times. Entities can be defined internally within the XML document or externally, referencing data from an outside source.

For example, an external entity references data from an external file or resource. In the following code, the entity `&ext;` could refer to an external file located at `<URL>` which would be loaded into the XML if allowed by the system.

```xml
<!DOCTYPE people [
   <!ENTITY ext SYSTEM "http://tryhackme.com/robots.txt">
]>
<people>
   <name>Glitch</name>
   <address>&ext;</address>
   <email>glitch@wareville.com</email>
   <phone>111000</phone>
</people>
```

External entities is one of the main reason that XXE is introduced if not managed properly.

**XML External Entity (XXE)**

XXE is an attack that takes advantage of **how XML parsers handle external entities.** When a web application processes an XML that contains an external entity, the parser attempts to load or execute whatever resource the entity points to. If necessary sanitisation is not in place, the attacker may point the entity to any malicious source/code causing the undesired behavior of the web app.

```xml
<!DOCTYPE people[
   <!ENTITY thmFile SYSTEM "file:///etc/passwd">
]>
<people>
   <name>Glitch</name>
   <address>&thmFile;</address>
   <email>glitch@wareville.com</email>
   <phone>111000</phone>
</people>
```

Entity is `&thmFile;` refers to the sensitive file `/etc/passwd` on a system. When XML is processed, the parser will try to load and display the contents of that file, exposing sensitive information to the attacker.

**Mitigation**

- **Disable external entity loading:** Primary fix. In PHP, can prevent XXE by setting **`libxml_disable_entity_loader(true)`** before processing the XML.
- **Validate and Sanitise User Input**: Always validate and sanitise the XML input received from users. This ensures that only expected data is processed, reducing the risk of malicious content being included in the request. For example, remove suspicious keywords like **`/etc/host`**, **`/etc/passwd`**, etc, from the request.

(Additional)

**AJAX Call (Asynchronous Javascript and XML)**

A method used to send or retrieve data from a server without reloading the web page, allowing for a more dynamic and interactive user experience by updating only parts of the page instead of the entire page.

1. Key characteristics
    1. Asynchronous: User can continue interacting with the web page while request is being processed
    2. Data formats: Data is often exchanged in formats like JSON, not just XML
    3. Browser-Server communication: Uses HTTP requests to communicate with the server and update the webpage dynamically
2. How AJAX Works
    1. Client-Side
        1. A JS function initiates an AJAX call
        2. Creates an XMLHttpRequest or uses modern APIs like `fetch()` or `axios`
    2. Service-Side
        1. Server processes the request (eg fetches data from a database)
        2. Sends a response back to the client in a specific format (eg JSON or XML)
    3. Client-Side Update:
        1. Response is processed and the webpage is updated without a full reload
3. AJAX Workflow
    1. User interacts with webpage (eg click a button)
    2. JS sends a request to the server
    3. Server processes the request and sends back data
    4. JS updates webpage dynamically with server response
4. Common Uses
    1. Search Autocomplete: Typing in a search box fetches suggestions dynamically
    2. Form Validation: Validating user input without reloading page
    3. Dynamic Content Loading: Loading posts, comments, or other data on-demand

**Changelog file**

A document that records all the changes made to a project. Widely used in software development to keep track of updates, fixes, enhancements and other modifications.

- **Structure of a Changelog**
    
    A typical changelog contains:
    
    - **Version Number**: Identifies the version or release.
    - **Release Date**: Specifies when the version was released.
    - **Change Categories**: Groups changes into categories such as:
        - **Added**: New features or functionality.
        - **Changed**: Updates to existing features.
        - **Deprecated**: Features that are being phased out.
        - **Removed**: Features that have been removed.
        - **Fixed**: Bugs that were resolved.
        - **Security**: Security vulnerabilities addressed.
