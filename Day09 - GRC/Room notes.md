Governance, Risk and Compliance (GRC) plays a crucial role in any organisation to ensure that their security practices align with their personal, regulatory and legal obligations. Although in general, good security practices help protect a business from suffering a breach, depending on the sector in which an organisation operates, there may be external security regulations that it needs to adhere to. 

![da9-1](https://github.com/user-attachments/assets/45b8721f-9a2b-4435-a8c6-f57fa36fc62e)

**Examples in the financial sector:**

- **Reserve Bank Regulations**: In most countries, banks have to adhere to the security regulations set forth by the country’s reserve bank, which ensures that each bank adheres to a minimum security level to protect the funds and information of their customers
- **SWIFT CSP**: Banks use SWIFT network to communicate with each other and send funds. After a [massive bank breach resulted in a $81 million fraudulent SWIFT transfer](https://www.wired.com/2016/05/insane-81m-bangladesh-bank-heist-heres-know/), SWIFT created the Customer Security Programme (CSP) which sets the standard of security for banks to connect to the SWIFT network.
- **Data Protection**: As banks hold sensitive information about their customers, they have to adhere to the security standards created by their data regulator (usually the reserve bank in most countries)

GRC plays a crucial role in understanding external security standards, translating them into internal standards and then ensuring that they are applied by all teams to help reduce the organisation’s risk to an acceptable level, when running a large organisation with multiple teams

**Governance**

- The function that creates the framework that an organisation uses to make decisions regarding information security.
- The creation of an organisation’s security strategy, policies, standards, and practices in alignment with the organisation’s overall goal.
- Defines the roles and responsibilities that everyone in the organisation has to play to help ensure these security standards are met

**Risk**

- The function that helps to identify, assess, quantify and mitigate risk to the organisation’s IT assets.
- Helps organisation understand potential threats and vulnerabilities and impact that they could have if a threat actor were to execute or exploit them
- Is important to help reduce the overall risk to an acceptable elvel and develop contingency plans in the event of a cyber attack where a risk is realised

**Compliance**

- The function that ensures that the organisation adheres to all external legal, regulatory and industry standards.

**Risk Assessments**

A process to identify potential problems before they happen.

A risk register tracks the progress of risk mitigation and all open risks.

![09SS01](https://github.com/user-attachments/assets/ad4c6936-36b9-43e0-a130-89af5efba73f)

**Identification of risks**

1. Identify factors that can cause revenue or reputation loss resulting from cyber threats
    1. Unpatched web server
    2. High-privileged user account without proper security controls
    3. Third-party vendor who might be infected by a malware connecting to the organisation’s networ
    4. A system for which support has ended by the vendor and still in production
    
    Risks also needs to be quantified. The likelihood of materialising a risk on a cordoned-off and isolated server differs greatly from that of a public-facing server hosting a web frontend or the impact of a risk materialising on a main database server containing confidential information differs greatly from that of a development server with dummy data
    
2. Assign likelihood to each risk
    
    To quantify risk, we need to identify how likely or probable it is that the risk will materialise, then assign a number to quantify this likelihood.
    
    Might differ from organisation to organisation and from framework to framwork
    
    1  **Improbable**: Unlikely that it might never happen
    
    2 **Remote**: Very unlikely to happen but still got possibility
    
    3 **Occasional:** Likely to happen once/sometime
    
    4 **Probable**: Likely to happen several times
    
    5 **Frequent:** Likely to happen often and regularly
    
    While we are trying to quantify the risk, we still don’t define exact quantities of what constitutes several times and what constitutes regularly as the likelihood for a server which has very high uptime requirements will be different from a server that is used infrequently hence the scale will differ.
    
3. Assigning impact to each risk
    
    Quantify the impact this risk’s materialisation might have on the organisation. Eg. if a public-facing web server that is unpatched and gets breached, what will be the impact on the organisation? 
    
    Organisations calculate impact in different ways, eg CVSS scoring, own rating derived from CIA etc
    
    1 **Informational**: Very low impact, almost non-existent
    
    2 **Low**: Impacting a limited part of one area of the organisation’s operations with little to no revenue loss
    
    3  **Medium**: Impacting one part of the organisation’s operations completely, with major revenue loss
    
    4 **High:** Impacting several parts of the organisation’s operations, causing significant revenue loss
    
    5 **Critical**: Posing an existential threat to the organisation
    
4. Risk Ownership
    
    Decide what to do with the risks that were found.
    
    Simplest way to calculate the risk is take the likelihood of the risk and multiply it with the impact of the risk to get a score.
    
    Some more advanced rating systems DREAD (A system used by microsoft to assess risk to computer security threat)
    
    While the simplest answer is to secure the system so there is no risk, in real life, implementing more security costs more money and doesn’t help if more money is spent on security than what is risked losing if the risk is open.
    
    Decides who owns the risks that were identified. The team members are then responsible for performing an additional investigation into what the cost would be to close the risk vs what could be lost if the risk is realised. If cost of security is lower, risk can be mitigated with more security controls. If higher, acccept the risk but accepted risks should always be documented and reviewed periodically to ensure that the cost has not changed
    

**Internal and Third-party Risk Assessments**

Risk assessments can also be used to assess the risk that a third party may hold to the organisation eg outsource - the risks change as a compromise of the third party as we may not have full control over their security, which could result in a compromise of data or sensitive assets.

Internal risk assessments help companies understand the risks they have within their own walls

- Identify weak spots in security
- Direct resources to most important areas
- Stay ompliant with security rules and regulations

Third Parties risk assessment

- Have good security measures to keep data safe
- Follow data protection rules
- Align with security standards that the organisation have in place
