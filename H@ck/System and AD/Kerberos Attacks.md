# Roasting, Tickets and Delegation

Kerberos attack references for SPN discovery, Kerberoasting, AS-REP roasting, pass-the-key, forged tickets, delegation and ticket handling.

**What are Kerberos Attacks?**

Kerberos authenticates identities using tickets issued by a trusted domain controller. Attack paths arise from weak service-account secrets, missing preauthentication, delegation rights or compromised ticket-signing material. Inspect the relevant identity and directory permissions before interpreting a ticket or delegation finding.

Windows commands run in the current logon session. Directory queries require the ActiveDirectory PowerShell module and access to a domain controller.