# Incident Response Documentation on Findings: Security Analyst 

# Introduction 
I assumed the role of Cyber Incident Responder within my Security Operations Center (SOC) investigating four security events utilizing NIST 800-61 Computer Incident Handling Guide. 

![image](https://github.com/VanessaMancia/Incident-Response-Documentation/assets/112146207/3940d956-05da-4945-8b03-67afeb07303d)
---
# Scope
4 Incidents have been detected and will be evaluated to see if they are true positive, true negative, or false positive false negative. 

* Incident ID:130 - Brute Force SUCCESS - Windows
* Incident ID:55 - Possible Privilege Escalation (Azure Key Vault Critical Credential Retrieval or Update)
* Incident ID:152 - Brute Force ATTEMPT - Linux Syslog
* Incident ID:18 Malware Detected

---
# Preparation:
* Log Analytics workspace is a central repository for collecting, analyzing, and visualizing logs from various sources. We then created a watchlist in Sentinel and ingested the GeoIP data, which was used to plot attackers on a map. Microsoft Defender for Cloud (MDC) was enabled, which gives insight into things. MDC allows us to take logs from VMs and forward them into the log analytics workspace. MDC automatically collects and forwards security logs to the log analytics workspace.

---
# NIST 800-53 Computer Incident Response Handling Guide  

## Incident ID: 130 - Brute Force SUCCESS - Windows

![Brute Force Success - Windows](https://github.com/VanessaMancia/Incident-Response-Documentation/assets/112146207/8424006b-e6bc-4dc3-af4e-10c740812759)

## Findings 
* 113.186.155.64 - Attacker / Entity 
* Details: City: Viet Tri
* Country: Vietnam 
* State: Phu Tho
* Continent: Asia

* Attacker/entity 113.186.155.64 is also involved in 2 other brute force successes and 7 brute force attempt instances
* Potentially compromised system "windows-vm" involved in several other incidents/alerts. Suspect possible over-exposure to the public internet.

## Post Incident Activities 
I wanted to see if this was a true positive. I inspected actions from 113.186.155.64 and there were 2 "successes." Upon further inspection, it was found that the alert raised was a false positive. After the "successes," the attacker continued attempts at brute forcing the system. Though the alert was a false positive, this type of traffic should not be reaching the VM in the first place. 

Closing out the incident as a false-positive will start the processes for hardening NSGs.

---

## Incident ID: 55 - Possible Privilege Escalation (Azure Key Vault Critical Credential Retrieval or Update)

![Possible Privilege Escalation (Azure Key Vault Critical Credential Retrieval or Update)](https://github.com/VanessaMancia/Incident-Response-Documentation/assets/112146207/b9740152-4ce6-4389-b3d3-35052e092bf1)


## Detection & Analysis
* Attacker/entity 72.204.16.50 attempted to view key vault credentials. 

* The user: Vanessa Mancia

* Not only did this user view the critical credentials many times, but they also were involved in several other incidents including excessive password resets and global admin role assignments. 

## Post Incident Activities: 
After calling the above user, they confirmed that they were just doing their normal duties, corroborating with their manager. Closing out for false positives. 

---

## Incident ID: 152 - Brute Force ATTEMPT - Linux Syslog 

![CUSTOM Brute Force ATTEMPT - Linux Syslog](https://github.com/VanessaMancia/Incident-Response-Documentation/assets/112146207/b9f4823f-4648-4f48-86e0-3e48105dd0c9)

## Detection & Analysis
* 218.92.0.76 - Attacker / Entity
* Organization: Chinanet Jiangsu Province Network
* City: Nanjing
* Country: China
* State: Jiangsu
* Continent: Asia 

2/10/2024 4:53 PM
* 218.92.0.113
* Organization: Chinanet Jiangsu Province Network
* City: Nanjing
* Country: China
* State: Jiangsu
* Continent: Asia

## Post Incident Activities
Both attackers at IP addresses 218.92.0.76 and 218.92.0.113 attempted many brute force attempts but weren't successful.  
The account was local to the Linux machine, non-admin, and essentially low impact. However, both attackers participated in brute force attacks.``` This demonstrates the need to continue the practice of a strong password policy, lock out accounts after a defined number of incorrect password attempts, and implement and enforce MFA. NSGs must be locked down as well.```

---

## Incident ID 18 Malware Detected

## Detection & Analysis
* This happened around 02/10/24 12:53 AM
* Several other alerts were raised for activities this VM is involved with 

## Post Incident Activities
* Corroborated with user and user's manager 
* As far as malware goes, this alert was a false-positive because it looks like the user was testing with EICAR files. Here is the query we used:

```
Event
| where EventLog == "Microsoft-Windows-Windows Defender/Operational"
| where EventID == "1116" or EventID == "1117"
| where RenderedDescription !has "EICAR"```








