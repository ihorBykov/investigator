---
title: Vidar MaaS Attribution
tags:
  - malware
  - vidar
  - stealer
  - maas
  - attribution
  - threat-intelligence
  - cybercrime
  - osint
type: investigation
status: completed
confidence: high
investigation-type:
  - threat-actor-attribution
  - malware-investigations
methodologies:
  - behavioral-attribution
  - infrastructure-artifact-analysis
  - osint-deanonymization
related-notes:
  - operational-security-failures
tools:
  - intelx
  - virustotal
  - telegram
  - maltego
  - python
---
## Executive Summary

This investigation focuses on the **Vidar Malware-as-a-Service (MaaS)** ecosystem, including its operators, infrastructure, and distribution channels.


The analysis identified:

- Core actors behind the service
- Supporting roles within the operation
- Infrastructure used for distribution and control
- Monetization model and operational maturity

**Key outcome:**  
Vidar operates as a **structured cybercriminal service**, with clear role separation, infrastructure reuse, and ongoing development (Vidar 2.0).

**Confidence level:** High  
**Threat level:** High

---
## Investigation Scope

**Objectives:**

- Identify actors behind Vidar MaaS
- Analyze infrastructure and distribution channels
- Map relationships between identities and assets
- Collect actionable IOCs

---

## Methodology

The investigation combined:

- Forum intelligence (underground communities)
- Infrastructure analysis (domains, WHOIS)
- Identity pivoting (nicknames → emails → domains)
- Malware ecosystem tracking
- IOC aggregation and validation

---

## Key Findings

### 1. MaaS Positioning & Evolution

- Vidar has been active since **2018**
- Operates under a **subscription model**
- Continuous development → release of **Vidar 2.0 (2025)**

Capabilities include:

- Browser data extraction
- Crypto wallet theft
- Card data harvesting
- Telegram session hijacking
- File exfiltration

This positions Vidar as a **highly mature stealer platform**.

---
### 2. Distribution Channels

Primary promotion channels:

- Underground forums:
    - exploit.in
    - xss.is
    - wwh-club
    - carder.market
- Social engineering vectors:
    - TikTok-based malware delivery campaigns

Indicates **multi-channel acquisition strategy**.

![[vidar_operational_ecosystem_mapping.png]]
## Operational Ecosystem Mapping

The investigation identified a structured operational ecosystem involving:
- service operators
- infrastructure components
- affiliate-facing distribution channels
- monetization workflows

The observed architecture demonstrated characteristics consistent with mature Malware-as-a-Service operations.

The investigation identified long-term operational presence across multiple underground communities, including sales activity, customer engagement, and service updates associated with the Vidar MaaS ecosystem.

Observed activity demonstrated:
- sustained operational continuity
- active customer acquisition
- ongoing product maintenance
- structured commercialization

![[vidar_forum_advert.png]]

The observed forum activity demonstrated sustained operational continuity and supported the assessment that Vidar operated as a mature commercialized malware service rather than isolated malware distribution activity.

Suggested highlights:  
- operator nickname  
- subscription pricing  
- Vidar branding  
- release/update announcements  
- support/contact methods
---
### 3. Core Actors

#### Primary Actor:

- Alias: `Loadbaks`
- Additional alias: `Sultan`
- Role: Product owner / operator

Key indicators:

- Created original sales thread (2018)
- No unrelated activity → dedicated account

---
#### Supporting Actor:

- Alias: `Nezvezda`
- Role: Support / communications

Indicators:

- Responded to customers
- Posted updates (2018–2019)

---
### 4. Infrastructure Analysis

#### Domains:

- `vidars[.]su`
- `vidar[.]su`
- `my-vidar[.]com`
- `vidar[.]news`

#### WHOIS-linked emails:

- `t***1@protonmail.com`
- `d***s@otmail.top`
- `a***l@superpost.xyz`

#### Key insight:

- Email registration timeline correlates with forum activity launch

Strong indicator of **single operator control or tightly coupled team**

---
### 5. Identity Pivoting

From domain registration:

- Email → additional domains
- Email → registration timestamps
- Email → cross-platform reuse

Example:
t***1@protonmail.com → my-vidar.com → related domains

Classic **operator fingerprinting via infrastructure reuse**

---
### 6. Command & Control (C2) Structure

Identified:

- Admin panel (builder, logs, worker sections)
- Authentication endpoints
- Controlled access via invite links

Recovered administrative interfaces provided visibility into the operational backend used to manage malware distribution, log aggregation, and affiliate-oriented workflows.

The observed functionality demonstrated characteristics consistent with mature MaaS infrastructure management.
![[vidar_c2_panel.png]]

The recovered backend infrastructure further reinforced the assessment of operational maturity, including role separation, centralized management capabilities, and scalable distribution workflows.

![[vidar_c2_admin_panel 1.png]]

Identified:  
- admin panel  
- builder interface  

Indicates:
- Structured backend
- Client-oriented service model
- Scalable operations

---
### 7. Monetization Model


The subscription-based pricing structure, combined with dedicated support channels and continuously maintained infrastructure, indicated a commercially mature cybercriminal service model rather than isolated malware distribution activity.

- 7 days — $130
- 30 days — $300
- 90 days — $750

Payment:

- BTC only

Strong indicator of:

- SaaS-like criminal model
- Stable customer base

---
### 8. Indicators of Compromise (IOCs)

#### Infrastructure:

- IP addresses (C2 nodes)
- Malicious URLs
- Payload delivery links

#### Communication:

- Telegram channels
- Jabber accounts

#### Financial:

- BTC wallet:

1PY4JX82rhKTSyP7ywhJgiYeVvTcpcaW8d

Enables:

- Detection
- Threat hunting
- Blocking

---

### 9. Attribution

Identified real-world entity:

- Name: _[REDACTED]_
- Location: Russia (Moscow)
- Phone: +7**********
- Linked via:
    - Domain registration
    - Contact reuse
    - Cross-platform correlation

---
## Evidence Logic

Attribution is based on:

**1. Infrastructure linkage**

- Shared emails across domains
- WHOIS correlation

**2. Identity reuse**

- Nicknames across forums
- Jabber accounts

**3. Temporal alignment**

- Email creation vs product launch

**4. Role consistency**

- One actor → product
- One actor → support

Combined signals → **high-confidence attribution**

## Threat Actor Role Separation

The observed ecosystem demonstrated clear role separation between:
- product development
- operational support
- customer communications
- infrastructure management

Such compartmentalization is commonly associated with mature cybercriminal service operations.

---
## Risk Assessment

**Threat Level:** High

- Widely distributed malware
- Active development lifecycle
- Multi-channel infection vectors
- Financially motivated actors

---

## Visualization (Process + Attribution Flow)

![[vidar_investigation_flow.png]]

---
## Operational Security Assessment

The investigation identified multiple operational security weaknesses, including:
- infrastructure reuse
- repeated identity artifacts
- email correlation across services
- temporal overlap between registrations and operational activity

These indicators significantly strengthened attribution confidence.

---
## Conclusion

The investigation demonstrates that Vidar is:

- A mature MaaS platform
- Operated by a structured group
- Supported by reusable infrastructure
- Actively evolving and scaling

**Final assessment:**  
Vidar represents a **persistent and scalable cybercrime threat** with strong operational discipline.

## Key Indicators & Infrastructure Artifacts were collected on May 26-27, 2025:
  
### Distribution Infrastructure  
- `http://94.159.105.149/12345.txt`  
- `https://app.box.com/shared/static/lr9c3ado06l5gfgzh1wq0ypp6efit0ay.zip`  
- `https://verif.rabonet.xyz/`  
  
---  
  
### Command & Control Infrastructure  
  
#### HTTPS Infrastructure  
- `https://116.203.164.14/`  
- `https://116.203.2.210/`  
- `https://116.203.165.251/`  
- `https://78.47.170.151/`  
- `https://116.202.6.61/`  
- `https://116.203.12.35/`  
#### Network Endpoints  
- `116.203.164.14:443`  
- `78.47.170.151:443`  
- `116.203.2.210:443`  
- `116.202.6.61:443`  
- `116.203.165.251:443`  
- `78.47.223.238:443`  
- `49.13.1.124:443`  
- `116.203.12.35:443`  
  
---  
### Domains & Subdomains  
- `ae.7.4t.com`  
- `https://ae.7.4t.com/`  
- `https://d3.7.4t.com/`  
  
---  
### Messaging Infrastructure  
- `https://t.me/eom25t`  
  
---  
### File & Malware Hashes  
  
#### SHA256  
- `3561b5b3972012641075bec94d5e9783ddb71aa9fc32762a09d80fcaf074bd2a`  
  
#### MD5  
- `a8da43f44370f0c6908fa01beab2415b`  
  
#### SHA1  
- `351b6e4451b2e55f2ee86aa737334bd6abededf4`  
  
---  
### Observations  
  
The identified infrastructure demonstrated:  
- rotating C2 infrastructure  
- multi-host deployment patterns  
- mixed IP/domain operational overlap  
- Telegram-based operational communication  
- staged payload distribution via external hosting services

--- 
## Related Methodologies:  

- [[infrastructure_artifact_analysis]]
- [[behavioral_attribution]]  

--- 
## Investigation Types

- [[malware_as_a_service]]  

---
## Notes

- [[operational_security_failures]]
