---
title: $4.5M Insider Crypto Fraud Investigation

tags:
  - blockchain-investigations
  - insider-threat
  - cryptocurrency
  - tron
  - bitcoin
  - ethereum
  - laundering
  - osint
  - attribution
  - fraud

type: investigation
status: completed
confidence: high

investigation-type:
  - blockchain-investigations
  - fraud-investigations

methodologies:
  - wallet-attribution
  - behavioral-attribution
  - infrastructure-artifact-analysis

related-notes:
  - operational-security-failures
  - tron-laundering-observations

tools:
  - trm-labs
  - arkham
  - python
  - osint
---
> End-to-end investigation of a multi-year insider laundering scheme using blockchain analytics, OSINT, and exchange attribution.

---
## Executive Summary

I led an investigation into a series of unauthorized withdrawals totaling approximately **$4.5 million**, affecting a crypto-enabled financial product over a 5-year period.

The case evolved from an initial hypothesis of external compromise into a confirmed **insider fraud operation**, involving structured fund laundering across multiple blockchains, the use of centralized exchanges for off-ramping, and identity exposure through OSINT and financial leakage.

The investigation resulted in **confirmed attribution via exchange KYC data**, enabling the client to proceed with legal action.

---
## My Role

- Led end-to-end investigation  
- Performed blockchain tracing and clustering (~70 wallets)  
- Identified laundering patterns and aggregation points  
- Conducted OSINT-based de-anonymization  
- Correlated on-chain activity with real-world identity  
- Supported VASP engagement process  

> Focus: combining technical analysis with investigative reasoning to reach attribution

---
## Problem

The client reported recurring unauthorized withdrawals with no clear compromise vector.

### Key Facts:

- **Total loss:** ~$4.5M  
- **Timeframe:** 2021–2026  
- **Wallets involved:** ~70  
- **Networks:** BTC, ETH, TRON (USDT), WebMoney  

The primary objective was to:

- Identify the laundering scheme  
- Trace fund flows  
- Determine attribution  

---
##  Initial Data

The investigation started with a structured dataset provided by the client:
- Total amount USDT
- Last transaction date
- Account user email
- Phone number
- IP
- Destination address

![[insider_threat_initial_dataset_example.png]]

---
## Timeline

2019 → Initial withdrawals
2022 → Expansion of laundering activity
2024 → Increased TRON usage
2026 → Investigation initiated

--- 
##  Investigation Approach

### 1. Blockchain Analysis

- Clustered wallets based on transaction relationships  
- Mapped multi-chain transaction flows to identify recurring fragmentation and consolidation behavior
- Identified central aggregation nodes  

---

### 2. Pattern Detection

A consistent laundering pattern was identified:

**Source → Split (10–20 wallets) → Merge → Exchange cash-out**
![[usdt_consolidation_tracing.png]]
[View Full Resolution Graph](graphs/usdt_consolidation_tracing.png)

_TRM  graph showing:_
- fragmentation (multiple wallets)  
- consolidation (single node)  
- flows to exchanges  
_Label stages: "Fragmentation", "Consolidation", "Cash-out"._

> Analyst Note:  
> Repeated split-and-merge behavior strongly suggested deliberate laundering rather than opportunistic theft.

---

### 3. VASP Attribution

Funds were traced to the following exchanges: Binance, Bybit, WhiteBIT, Bitget, KuCoin, Rapira.
Bybit  deposit wallet received ~$2.3M.

![[insider_fraud_deposit_wallet.png]]
This address was a deposit address and identifying information was obtained during LA's cooperation with the exchange.

**Key Finding:**

- ~68% of traced funds moved through TRON
-  23 primary aggregation wallets identified
- ~$2.3 M routed through a single deposit address
- A Bybit-linked deposit address was identified, and KYC-associated account information was obtained during legal cooperation with the exchange
- ~600 traced transactions analyzed
- 42 aggregation-related wallet clusters identified

---

### 4. OSINT De-anonymization

A critical off-chain linkage was identified:

- By analyzing information leaked by the third-party service obmen4you and verifying information on compromised accounts, we were able to identify the card number, and subsequently the cardholder.
- The wallet appeared in a leaked exchanger database  
- The dataset contained a transaction funded via bank card 

 **`[REDECTED]]@yandex.ru` (`Z1**********8`), data leak of service obmen4you.**

**Process:**
1. Extracted bank card reference  
2. Verified ownership via banking application  
3. Obtained partial identity (name + initial)  

---

## Key Investigative Breakthrough

The investigation pivoted after identifying a WebMoney wallet referenced in a leaked exchanger dataset.
Correlation of leaked transaction records with bank card funding activity enabled partial identity resolution, ultimately linking the laundering infrastructure to an internal employee.

![[insider_fraud_identity_attribution.png]]
--- 

### 5. Attribution

Correlation of blockchain and OSINT findings resulted in a full attribution chain:

**Webmoney Wallet → Leak → Bank Card → Name → Employee**
![[insider_fraud_webmoney.png]]
[View Full Resolution Graph](graphs/4.5KK_fraud_actor_attribution.png)

 Investigative Outcome:
- Identified internal employee  
- Role: Team Lead (Anti-Fraud Testing) 

> This confirmed insider involvement rather than external compromise.

## Attribution Confidence  
  
High confidence attribution based on:  
- exchange KYC records  
- wallet flow continuity  
- OSINT correlation  
- financial linkage through bank card records
---

## Key Decisions & Turning Points

### Shift in Hypothesis
Initial assumption of external compromise was revised to insider threat based on:
- Transaction consistency  
- Structured execution patterns  

---

### Exploiting OSINT Signal

The investigation leveraged leaked exchanger data containing historical WebMoney transaction records.
One wallet associated with the laundering flow was linked to a bank card funding transaction, enabling partial identity resolution and subsequent correlation with an internal employee.

---

### Focus on Aggregation Nodes

High-value consolidation wallets were prioritized to:
- Maximize attribution probability  
- Identify VASP exposure points  

---

## Adversary Tradecraft & Mistakes

### Techniques Used:
- Fund fragmentation across multiple wallets  
- Cross-chain transfers  
- Use of centralized exchanges for cash-out  

---

### Critical Mistakes:

- Reuse of WebMoney wallet present in leaked dataset  
- Traceable fiat funding via bank card  
- Test transaction exposing wallet linkage  
- Consolidation into identifiable exchange endpoints  

![[insider_fraud_full_graph_pii.png]]
[View Full Resolution Graph](full_4.5KK_fraud_full_graph.png)

**Attribution Result:**
- Identified internal employee  
- The identified employee held a privileged anti-fraud role.

> This confirmed insider involvement rather than external compromise.

---

## Investigation Limitations

- Multiple exchanges did not respond to legal requests  
- Some historical transactions were no longer available  
- Several intermediary wallets could not be conclusively attributed

---

## Outcome

- Laundering scheme attributed to internal actor  
- Identity confirmed via exchange KYC  
- Evidence package delivered to client  

Client actions:
- Legal proceedings initiated  
- Asset freezing requests submitted  

---

## Impact

- Enabled attribution of ~$4.5M fraud  
- Identified insider threat vector  
- Provided actionable intelligence for legal enforcement  

---

## Key Takeaways

- Insider threats can operate undetected over long periods  
- TRON is commonly used for cost-efficient laundering  
- OSINT is often decisive for final attribution  
- Consolidation points are key investigative leverage  

---



- Multiple exchanges did not respond to legal requests
- Some historical transactions were no longer available
- Several intermediary wallets could not be conclusively attributed

---
## Tools & Skills Demonstrated

**Blockchain Analytics**
- TRM Labs    

**OSINT**
- Leak intelligence analysis (IntelX)
- Identity correlation  

**Technical**
- Data analysis  
- Pattern recognition  
- Cross-chain tracing  

**Investigative**
- Hypothesis testing  
- Attribution logic  
- Evidence building  
## Related Methodologies  
  
- [[wallet_attribution]]
- [[behavioral_attribution]]

---
## Related Notes

- [[operational_security_failures]]
- [[tron_laundering_observations]]

---
## Investigation Types

- [[insider_fraud]]
- [[blockchain_investigations]]