1. Analyse the flow of the application
    - Go through the steps as a user
  
2. Try intercepting with burp suite
    
    ![05screenshot01](https://github.com/user-attachments/assets/b44d981c-cfdc-4b73-b95d-bbb45bee9961)
    

When you visit the URL on the product page and click `Add to Wishlist`, an AJAX call (more information in the notes section) is made to `wishlist.php` with the following XML as an input.

```xml
<wishlist>
  <user_id>1</user_id>
     <item>
       <product_id>1</product_id>
     </item>
</wishlist>
```

After adding item to the wish list, the request logged in Burp Suite’s `HTTP history` contains the above XML being forwarded as a `POST` request.

![05screenshot02](https://github.com/user-attachments/assets/ead0c0df-9460-4e1b-956d-71adfb061b98)

The `wishlist.php` accepts the request and parses the request using the following code: (more information in the notes)

```xml
<?php
..
...
libxml_disable_entity_loader(false);
$wishlist = simplexml_load_string($xml_data, "SimpleXMLElement", LIBXML_NOENT);

...
..
echo "Item added to your wishlist successfully.";
?>
```

Additional notes:

`libxml_disable_entity_loader(false);` re-enables the loading of external entities, which is typically disabled by default for security.

`simplexml_load_string($xml_data, "SimpleXMLElement", LIBXML_NOENT);` 

The `LIBXML_NONENT` flag allows external entities to be processed, making it possible for an attacker to inject malicious XML data that references external entities.

3. Preparing the payload

When a user sends specially crafted XML data to the application, the line `libxml_disable_entity_loader(false)` allows the XML parser to load external entities. meaning the XML input can include external file references or requests to remote servers. When the XML is processed by `simplexml_load_string` with the `LIBXML_NOENT` option, the web app resolves external entities, allowing attackers access to sensitive files or allowing them to make unintended requests from the server.

Payload:

```xml
<!--?xml version="1.0" ?-->
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/etc/hosts"> ]>
<wishlist>
  <user_id>1</user_id>
     <item>
       <product_id>&payload;</product_id>
     </item>
</wishlist>
```

The first two lines introduce an external entity called **payload**. The line `<!ENTITY payload SYSTEM "/etc/hosts">` tells the XML parser to replace the `&payload` referenced with the `/etc/hosts` and the `product_id` referencing to the `payload` will read and display the contents of the file instead of the normal `product_id`.

4. Exploitation

Send to `Repeater` in Burp Suite that will allow sending of multiple HTTP requests. Use this feature to duplicate `HTTP POST` request and send it multiple times to exploit the vulnerability. 

![05screenshot03](https://github.com/user-attachments/assets/2da34026-146f-4808-9d86-391fabcf00e0)

Replace the XML in the `Repeater` with the above code.

<img width="422" alt="05screenshot04" src="https://github.com/user-attachments/assets/e6461d4c-3f4a-4bb0-818a-91b55055deef">

<img width="380" alt="05screenshot05" src="https://github.com/user-attachments/assets/92e39ac8-113e-4c5d-9b13-7a5679da8f25">

Replaced

5. Results
    
![05screenshot06](https://github.com/user-attachments/assets/13a75714-b042-4ba5-930f-112d9438cc16)

    
After clicking `Send` , the server processed the malicious XML payload, which included the external entity reference to `/etc/hosts`. Hence the `wishlist.php` responded with the contents of the `/etc/hosts` file, leading to an XXE vulnerability.
