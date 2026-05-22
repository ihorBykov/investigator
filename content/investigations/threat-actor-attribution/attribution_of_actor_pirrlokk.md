---
title: Attribution of Actor "Pirrlokk"

tags:
  - threat-actor
  - attribution
  - osint
  - underground-forums
  - deanonymization

type: investigation
status: completed
confidence: medium

investigation-type:
  - threat-actor-attribution

methodologies:
  - behavioral-attribution
  - osint-deanonymization

tools:
  - maltego
  - telegram
  - intelx
---
## Executive Summary

This case demonstrates a full-cycle OSINT investigation focused on **attributing a pseudonymous actor** operating across multiple platforms.

The investigation resulted in:

- Identification of cross-platform presence
- Correlation of communication and financial artifacts
- Detection of OPSEC weaknesses
- High-confidence attribution based on multi-source analysis

> **Disclaimer:**  
> All sensitive personal data has been anonymized or removed. This case is presented for portfolio and educational purposes only.

---

## Objective

To identify and attribute the real-world identity behind the nickname **"Pirrlokk"** using publicly available data and OSINT techniques.

---

## Investigation Flow

![[pirrlokk_full_diagram.png]]

---

## Methodology

The investigation was conducted using a structured OSINT approach:

### 1. Username Pivoting

- Enumeration of the nickname across platforms
- Detection of reused identities
- Expansion of actor footprint

### 2. Forum & Marketplace Analysis

- Analysis of activity on niche and underground forums
- Extraction of behavioral patterns and interaction style

### 3. Communication Correlation

- Identification of reused identifiers:
    - messaging handles
    - usernames across services
- Cross-linking accounts via shared contacts

### 4. Financial Artifact Analysis

- Detection of recurring payment identifiers
- Linking transactions and accounts across platforms

### 5. Identity & Media Analysis

- Reverse image search
- Detection of fake or duplicated identities
- Profile comparison across platforms

### 6. Contextual & Ecosystem Analysis

- Analysis of associated projects and services
- Mapping actor’s activity domain (gaming, crypto, freelance)

---
## Key Findings

### Cross-Platform Identity Reuse

The actor reused the same nickname across multiple services, enabling initial correlation.

---
### Communication Artifact Overlap

Multiple accounts were linked via shared communication identifiers, significantly increasing attribution confidence.

---

### Financial Linkage

Recurring financial artifacts (wallets, payment identifiers) were reused across platforms.

---

### OPSEC Weaknesses

The actor demonstrated several operational security flaws:

- identifier reuse
- cross-platform linkage
- inconsistent identity separation

---

### Use of Secondary / Fake Identities

Evidence suggests the use of fabricated or borrowed profiles to obscure real identity.

---

## Attribution Logic

The attribution is based on correlation of independent signals:

|Signal Type|Description|
|---|---|
|Username reuse|Same nickname across multiple platforms|
|Communication artifacts|Reused messaging identifiers|
|Financial artifacts|Shared wallets / payment methods|
|Behavioral patterns|Consistent activity type|
|Cross-platform overlap|Linked accounts across ecosystems|

**Conclusion:**  
The combination of these signals provides **high-confidence attribution** to a single actor.

---

## Evidence (Sanitized)

Examples of collected and processed data:

Username: Pirrlokk  
Telegram: @Pirrlokk  
Email: p*****k@****.com  
Wallet: 0xA12B****89F3

Additional sources:

- forum posts (archived)
- platform profiles (anonymized)
- communication artifacts (redacted)

---

## Tools & Techniques

- Username enumeration tools (e.g. Maigret-like approach)  
- Custom OSINT scripts (data correlation)  
- Archive services (historical snapshots)  
- Reverse image search  
- Manual intelligence analysis

---

## Analytical Approach

This investigation relied on:

- Multi-source correlation
- Pattern recognition
- Behavioral analysis
- Iterative hypothesis validation

The focus was not on isolated data points, but on **linking weak signals into strong attribution evidence**.

---

## Limitations

- Some data could not be independently verified
- Possible intentional deception by the actor
- Attribution is OSINT-based (not legal identification)

---

## Conclusion

This case demonstrates how fragmented digital traces can be aggregated to:

- reconstruct an actor’s digital footprint
- identify operational patterns
- achieve high-confidence attribution

---

## Skills Demonstrated

- OSINT investigation
- Threat Intelligence analysis
- Identity correlation
- Data enrichment & pivoting
- Analytical reporting

---

## Ethics & Privacy

- All sensitive data has been removed or anonymized
- No doxxing or direct exposure of private individuals
- Focus is on methodology, not personal data

---
## Methodologies

- [[behavioral_attribution]]

---
## Investigation Types

- [[threat_actor_attribution]]
--- 
## Notes

- [[common_attribution_mistakes]]
-  [[operational_security_failures]]