**DREAD (Risk Assessment Model)**

A model consists of quantifying the categories of Damage, Reproducibility, Exploitability, Affected Users and Discoverability with a rating of 0 to 10.

Initially proposed for threat modeling but abandoned when it was discovered that the ratings were not very consistent and are subjected to debate. Discontinued at Microsoft by 2008.

### **DREAD Category: Damage**

Damage relates to the potential damage (or impact) a threat could cause if it is exploited by an attacker.

The ratings consist of:

- 0, which is no damage
- 5, which is information disclosure
- 8, which is non-sensitive data of individuals being compromised
- 9, which is non-sensitive administrative data being compromised
- 10, which is destruction of the system in scope, the data, or loss of system availability

### **DREAD Category: Reproducibility**

Reproducibility relates to how easily (or likely) an attack can be replicated.

The ratings consist of:

- 0, which is nearly impossible or difficult
- 5, which is complex
- 7.5, which is easy
- 10, which is very easy

### **DREAD Category: Exploitability**

Exploitability relates to how easily or likely a weakness or threat can be exploited.

The ratings consist of:

- 2.5, which requires advanced technical skills
- 5, which requires tools that are available
- 9, which requires application proxies
- 10, which (only) requires browser

### **DREAD Category: Affected Users**

Affected Users relates to the number of (end) users that could be affected by a threat being exploited.

The ratings consist of:

- 0, which is no users are affected
- 2.5, which is only individual users are affected
- 6, which is few users are affected
- 8, which is administrative users are affected (these are more important users)
- 10, which is all users are affected

### **DREAD Category: Discoverability**

Discoverability relates to how likely a threat will be discovered by an attacker.

The ratings consist of:

- 0, which is hard to discover
- 5, which is open requests can discover the threat
- 8, which is a threat being publicly known or found
- 10, which is the threat is easily discoverable, such as in an easily accessible page or form

### Overall DREAD rating

- **Low** for overall ratings between **1-10**, which is a low risk to the application or system in scope.
- **Medium** for overall ratings between **11-24**, which is a medium or moderate risk to the application or system in scope.
- **High** for overall ratings between **25-39**, which is a high or severe risk to the application or system in scope.
- **Critical** for overall ratings between **40-50**, which is a critical risk to the application or system in scope.

### AES Encryption

Advanced Encryption Standard (AES)

[https://www.techtarget.com/searchsecurity/definition/Advanced-Encryption-Standard#:~:text=The Advanced Encryption Standard (AES) is a symmetric block cipher,cybersecurity and electronic data protection](https://www.techtarget.com/searchsecurity/definition/Advanced-Encryption-Standard#:~:text=The%20Advanced%20Encryption%20Standard%20(AES)%20is%20a%20symmetric%20block%20cipher,cybersecurity%20and%20electronic%20data%20protection).
