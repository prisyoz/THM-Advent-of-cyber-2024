**Certificates**

- Public key
    - The core of the certificate contains a public key, part of a pair of cryptographic keys: a public key and a private key.
    - Made available to anyone and is used to encrypt data
- Private key
    - remains secret and is used by the website or server to decrypt the data
- Metadata
    - Provides additional information about the certificate holder (website) and the certificate
    - Information about Certificate Authority (CA), subject (information about website), uniquely identifiable number, validity period, signature and hashing algorithm

**Certificate Authority**

A trusted entity that issues certificates eg GlobalSign, Let’s Encrypt, DigiCert.

The browser trusts these entities and performs a series of checks to ensure it is a trusted CA

Breakdown of what happens with a certificate:

1. **Handshake**
    1. Browser requests a secure connection, website responds by sending a certificate 
    2. in this case only requires public key and metadata
2. [**Verification**](https://www.sectigo.com/resource-library/dv-ov-ev-ssl-certificates)
    1. Browser checks the certificate for its validity by checking if it was issued by a trusted CA. If certificate hasn’t expired or been tampered with, and CA is trusted, browser gives the green light
3. **Key Exchange**
    1. Browser uses public key to encrypt a session key, which encrypts all communications between browser and website
4. **Decryption**
    1. Website (server) uses its private key to decrypt the session key, which is symmetric. 
    2. Both browser and website share a secret key (session key), a secure and encrypted communication is established

<aside>
💡

HTTP(S) is secure because of certificates (authentication, encryption and data integrity)

</aside>

**Self-signed certificates vs Trusted CA certificates**

- Process of acquiring a certificate with a CA is long - create the certificate, send it to a CA to sign it for you
    - If no tools and automation in place, this process can take weeks
- Self-signed certificates are signed by an entity, usually the same one that authenticates
    - Eg Wareville owns the GiftScheduler site. If they create a certificate and sign it with Wareville as a CA, it becomes a self-signed certificate
- Browsers generally do not trust self-signed certificates as there is no third-party verification.
    - Has no way of knowing if the certificate is authentic or if it’s being used for malicious purposes (MITM attack)
- Trusted CA certificates are verified by a CA which acts as a trusted third party to confirm the website’s identity
