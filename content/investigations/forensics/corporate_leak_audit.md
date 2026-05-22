---
title: Corporate Leak Audit Report

tags:
  - leaks
  - corporate-security
  - stealer-logs
  - credentials
  - osint
  - threat-intelligence

type: investigation
status: completed
confidence: high

investigation-type:
  - leak-analysis

methodologies:
  - infrastructure-artifact-analysis
  - osint-deanonymization

tools:
  - python
  - elasticsearch
  - osint
---
## 1. Executive Summary

### 1.1. Introduction

On October 04, 2024, while analyzing information collected using OSINT techniques, it was discovered that some sensitive data belonging to the organization “domain.ko.uk” is available
on both the Clearnet and DarkNET segments of the Internet.
In the course of verifying the domain information of the company domain.ko.uk, links and access credentials were collected from both DarkNET and other resources known to be
used by hackers.

### 1.2. The objectives of this assessment were:

- detecting compromising information relating to domain.ko.uk;
- searching for possible leaked credentials that could be used by malicious hackers to access domain.ko.uk resources;
- searching for possible leaked customer account information;
- searching for information about possible cyberattacks on domain.ko.uk network resources;

### 1.3. Investigation summary

>[!info] As a result of the check, we came to the following conclusion:

| Category                    | Description                                                                                                     | Discovered                                  |
| --------------------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| Compromising<br>Information | Information of a<br>compromising nature<br>about the<br>customer/company or its<br>representatives              | <span style="color:green;">NOT FOUND</span> |
| Leaked<br>Credentials       | Leaked credentials that<br>attackers could<br>use to access the<br>network resources of<br>the customer/company | <span style="color:red;">FOUND</span>       |
| Leaked<br>Credentials       | Leaked customer account<br>information                                                                          | <span style="color:green;">NOT FOUND</span> |
| Possible Cyberattacks       | Information on possible<br>cyberattacks targeting the IT<br>infrastructure of the<br>customer/company           | <span style="color:green;">NOT FOUND</span> |
## 2. Research methodology

### 2.1 Searching for accounts and information that could be used to attack the organization's domain.ko.uk infrastructure.

Using various leak aggregation resources, as well as the
DarkNET forums, an extensive analysis was conducted to find
the following information:
- Possible compromising information about domain.ko.uk;
- Customer account information about domain.ko.uk;
- Corporate accounts of domain.ko.uk employees;
- Information about pending or completed cyberattacks on the domain.ko.uk IT infrastructure.
## 3. Findings
### 3.1. Employee Credentials Leak (Critical) 

Sources: Clearnet and DarkNET, as well as other resources known to hackers.
OSINT research revealed the following list of corporate email addresses and passwords or hashed passwords. Below an excerpt of the information found:
![[corporate_leak_audit.png]]

Impact: Corporate credentials available in the DarkNET database can be used by potential attackers to gain access to corporate network assets.

---
## Investigation types

- [[leak_analysis]]

---
## Methodologies

- [[infrastructure_artifact_analysis]]

---
## Notes

- [[common_attribution_mistakes]]
- [[operational_security_failures]]