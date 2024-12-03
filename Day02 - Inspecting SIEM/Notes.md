## **True Positives or False Positives?**

In a SOC, events from different devices are sent to the SIEM. 

Certain rules(Detection Engineering rules) are defined to identify malicious or suspicious activity from these events. If an event or set of events fulfils the conditions of a rule, it triggers an alert.

SOC analyst then analyses the alert to identify if it is True Positive (TP) or False Positive (FP).

- TP if it contains actual malicious activity
- FP if it is not actually malicious

Can be difficult to differentiate between attacker and system administrator - to determine if it is TP or FP

**Decision Making**

If TP is falsely classified as an FP, can lead to significant impact from a missed cyber attack.

If FP is falsely classified as an TP, time ‘wasted’, less focus on an actual attack

- SOC can just confirm with the user, in which this privilege is not available to the attacker.
    - email or call the relevant person to get confirmation of a certain activity
    - any changes that might trigger an alert often require *Change Requests* to be created and approved through the IT change management process
    - Ask users to share *Change Request* details for confirmation, especially if it is a legitimate and approved activity
- Other cases that might hinder the investigation
    - If organisation does not have a change request process in place
    - Performed activity was outside the scope of the change request or was different from the approved change request
    - Activity triggered an alert, eg copy files to a certain location, uploading file to some website or failed login to a system
    - Insider threat performed an activity they are not authorised to perform, whether intentionally or unintentionally
    - User performed a malicious activity via social engineering from a threat actor
        - Understand the context of the activity and make judgement call
        - eg user from network team use Wireshark, other users from the same team might also use Wireshark. But if Wireshark is used on a machine belonging to HR or Finance should be suspicious

**Correlation**

- Correlate different events to make a story or timeline when building context. (Using past and future events)
- Note important artefacts (IP addresses, machine names, user names, hashes, file paths etc) to be used to connect the dots
- Create hypothesis and using evidence to support (proxy logs, website with spoofed domain name, file downloaded etc)
