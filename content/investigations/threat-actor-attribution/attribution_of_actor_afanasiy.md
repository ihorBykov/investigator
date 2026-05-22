---
title: Attribution of Actor "Afanasiy"
tags:
  - threat-actor
  - attribution
  - osint
  - deanonymization
  - cybercrime
type: investigation
status: completed
confidence: medium
investigation-type:
  - threat-actor-attribution
methodologies:
  - behavioral-attribution
  - osint-deanonymization
  - infrastructure-artifact-analysis
tools:
  - maltego
  - intelx
  - telegram
  - python
---
## Executive Summary

An OSINT investigation was conducted based on a single moniker.

The analysis revealed a **multi-layered digital identity** linked to underground financial fraud communities (carding). Through correlation of emails, communication handles, and behavioral patterns, multiple accounts across forums and platforms were attributed to a **single individual**.

**Key outcome:**

- Confirmed identity linkage across multiple aliases
- Established involvement in carding-related activities
- Identified real-world persona with supporting evidence

**Confidence level:** High  
**Risk level:** High

---
## Investigation Scope

**Initial input:**

- Moniker: `Afanasiy`

**Objectives:**

- Identify related accounts and infrastructure
- Assess credibility of underground activity
- Attribute online identities to a real individual

---
## Methodology

The investigation followed a structured OSINT approach:

- Identity pivoting (nickname → email → accounts)
- Leak and credential correlation
- Communication analysis (ICQ, Jabber, forums)
- Cross-platform identity linking
- Behavioral pattern analysis

---
## Key Findings

### 1. Initial Signal Analysis

The moniker `Afanasiy` was identified on an underground forum associated with data trading.

Observations:

- Low activity account (only a few posts)
- Suspicious sales pattern (instant “sold” response)
- Indicators of potential scam behavior
- Non-reputable forum selection for high-value data

This suggested **low credibility of the sale**, but not necessarily low expertise of the actor.

---
### 2. Email Correlation

A large dataset of emails (~90+) was collected based on moniker reuse.

After filtering:

- Only **one email** was directly linked to the forum account
- Key pivot email identified:

a***y@gmail.com

---

### 3. Identity Pivot: Email → Social Profile

The email was linked to a social media profile:

- Name: _[REDACTED]_
- Additional handle: `andyharmornd`

Supporting indicators:

- Reuse of nickname patterns
- Cross-platform presence
- Consistent identity artifacts

---
### 4. Underground Activity Attribution

The email and associated aliases were found on multiple underground platforms:

- Carding forums
- Fraud marketplaces
- Communication platforms (ICQ, Jabber)

Indicators:

- Purchase requests for stolen card data
- Participation in fraud-related discussions
- Repeated use of the same contact handles

Example pattern:

ICQ: 1\*\*\*312 
Jabber: o**@jabber-***  
Alias set: Andy / Opa / Afanasiy / Andy12

---
### 5. Credential Reuse & Leak Analysis

Multiple credentials were identified:

- Reused passwords across platforms
- Hashes linked to known accounts
- Cross-forum registration overlap

This enabled:

- Linking multiple accounts together
- Building a consistent identity cluster

---

### 6. Fraud Reputation Signals

One of the identified aliases was involved in arbitration on an underground forum:

- Accused of fraud (“scam” behavior)
- Account was banned
- Linked aliases confirmed via shared contacts

This significantly strengthens attribution confidence.

---

### 7. Deep Identity Expansion

Further pivots led to:

- Additional emails
- Forum accounts
- Communication IDs

A second identity cluster emerged:

m***m@mail.ru  
ICQ: 4\*\*\*29979

This cluster:

- Intersects with the original ICQ (1\*\*\*312)
- Shares behavioral and thematic overlap
- Operates in the same ecosystem

---

### 8. Real-World Attribution

Through OSINT pivoting (email → social networks → archives):

Identified:

- Full name: _[REDACTED]_
- Location indicators: Belarus (Brest / Minsk), Moscow (historical activity)
- Education: Business/Management (graduated ~2009)

Additional signals:

- Archived social media profiles
- Historical posts and connections
- Cross-referenced personal data

---

### 9. Phone Number Correlation

A phone number linked to one of the emails:

+375\*\*\*\*\*\*\*
Was used in:

- Classified platforms
- Social media posts
- Account registrations

This enabled:

- Further identity confirmation
- Linking to additional profiles

---

## Evidence Logic (Attribution Model)

The attribution is based on **multi-factor correlation**, not a single indicator:

**1. Shared communication identifiers**

- ICQ reused across multiple accounts
- Jabber contact appearing in different forums

**2. Credential reuse**

- Same passwords across platforms
- Email reuse patterns

**3. Behavioral consistency**

- Same activity domain (carding)
- Similar posting style and intent

**4. Cross-platform identity overlap**

- Forums ↔ social media ↔ marketplaces

**5. Temporal consistency**

- Activity timeline aligns across accounts

 No single data point is decisive, but **combined evidence creates a strong attribution chain**.

---

## Risk Assessment

**Fraud Risk:** High  
Subject actively engaged in financial fraud ecosystem.

**Data Exposure Risk:** High  
Involvement in buying/selling compromised financial data.

**Reputational Risk:** High  
Association with underground communities and scam reports.

---

## Visualization

> 🔗 Investigation graph:


![[afanasiy_full_diagram.png]]

---

## Conclusion

The investigation successfully:

- Attributed multiple underground identities to a single actor
- Confirmed involvement in carding-related activities
- Linked online presence to a real-world individual

**Confidence level:** High  
**Attribution type:** Multi-source OSINT correlation

---

## What Makes This Case Valuable

- Demonstrates deep identity pivoting
- Shows real-world attribution from minimal input
- Combines leaks, behavior, and infrastructure
- Applies analytical reasoning, not just data collection'

---
## Methodologies

-  [[behavioral_attribution]]
- [[infrastructure_artifact_analysis]]

---
## Investigation Types

- [[threat_actor_attribution]]

---
## Notes

- [[operational_security_failures]]