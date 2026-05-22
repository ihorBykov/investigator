---
title: Phobos Ransomware Investigation

tags:
  - ransomware
  - phobos
  - cryptocurrency
  - bitcoin
  - attribution
  - threat-intelligence
  - blockchain

type: investigation
status: completed
confidence: medium

investigation-type:
  - ransomware-investigations
  - blockchain-investigations

methodologies:
  - wallet-attribution
  - behavioral-attribution

related-notes:
  - ransomware-wallet-patterns

tools:
  - trm-labs
  - osint
  - virustotal
---
## Executive Summary

This investigation analyzes transactional and user data from a cryptocurrency exchange dataset associated with financial flows linked to the **Phobos ransomware ecosystem**.

By combining database analysis, blockchain tracing, and OSINT techniques, the research aimed to:

- Trace ransomware-related transactions
- Identify wallet ownership patterns
- Link blockchain activity to platform users
- Assess attribution reliability

---
### Key Findings

- Multiple **Phobos-attributed wallets** were linked to exchange-controlled addresses
- Identified **~10 exchange-associated wallets** receiving funds from ransomware activity
- Partial attribution achieved via **Telegram identity resolution (1/2 cases)**
- Detected **wallet reuse across multiple users**, indicating pooled infrastructure

---
### Critical Insight

> The exchange operates a **pooled wallet architecture**, preventing reliable 1:1 attribution between blockchain addresses and individual users.

## Strategic Insight

This case highlights a critical challenge in blockchain investigations:

> The presence of pooled exchange infrastructure fundamentally limits
> the reliability of wallet-based attribution.

Even when transaction flows and user mappings are identified,
ownership cannot be assumed without understanding platform architecture.

---
### Final Assessment

- **Attribution confidence:** Medium
- **Threat level:** High
- **Attribution reliability:** Constrained by platform architecture

---
## Investigation Scope

### Dataset

The analysis is based on a **restricted-access cryptocurrency exchange dataset** obtained during a controlled analytical engagement.

> ⚠️ Note: Data origin and acquisition methods are intentionally abstracted.

---
### Objectives

- Map transaction flows related to ransomware activity
- Identify relationships between wallets and platform users
- Validate identity attribution using OSINT
- Assess limitations of exchange-based attribution

---

## Methodology

- PostgreSQL deployment and database exploration
- Schema analysis via relational mapping (DBeaver)
- Transaction tracing using hash-based pivoting
- Wallet clustering and attribution (TRM-labeled data)
- Identity correlation via internal user records
- OSINT validation (Telegram, archive data)

---
## Key Findings

---
### 1. Target Selection

From the available dataset:

- Applied filter: **Ransomware category**
- Selected group: **Phobos** (highest activity segment)
- Total identified transactions: **408**

![[phobos_transactions_list.png]]

---

### 2. Data Processing & Schema Analysis

- Database deployed in PostgreSQL environment
- Accessed via DBeaver
- Schema explored using relational graph visualization

![[exchanger_database_related_tables.png]]
![[transaction_identification_tables.png]]

---
### 3. Transaction Analysis

#### Entry Point

Known transaction hash:

`f39b4cdfeca812c6d3201e4fa78b0a5bdc7cfe17b1bd25306e880d4364ca71e1`

#### Pivot Logic

Transaction Hash → blockchain_tx → withdrawal → wallet address

#### Example Queries

Entry point:
Known **transaction hash**: `f39b4cdfeca812c6d3201e4fa78b0a5bdc7cfe17b1bd25306e880d4364ca71e1`

![[phobos_transaction_identification.png]]

Information about transaction id: `3250617`

```sql
SELECT * FROM withdrawal  
WHERE blockchain_tx_id = 3250617;
```
 
  This query was used to identify the transaction details associated with the known transaction_id.  
  
Result:  
- Linked to withdrawal entry  
- Enabled pivot to wallet address

![[phobos_involved_transactions.png]]
All wallets involved in transaction, during analysis was established Phobos attributed address.
![[phobos_related_wallet.png]] 
 Analyzed attributed wallet: `bc1qre3v2yfl4hr4f8uza66k7jlraxvphc6eaylk46`
 
---
### Key Observation

- Withdrawal table contained relevant transaction data
- Deposit table was **empty**
![[exchange_database_table_list.png]]![[phobos_exchange_deposit_table.png]]
---
### Analytical Implication

> Absence of deposit data prevents full transaction lifecycle reconstruction  
> and limits inbound attribution capability.

---

### 4. Wallet Attribution

### Associated Wallet Correlation

#### Wallet: `bc1qre3v...`

Linked user records identified within leaked exchange-related datasets:

- **User ID:** `13461702`
  - associated exchange account metadata identified
  - correlation established through shared wallet reference

![[excange_database_user_info.png]]
---

- **User ID:** `13462376`
  - related exchange-linked operational record identified
  - additional attribution correlation observed

![[phobos_related_exchange_user_id.png]]

---
### Investigative Significance

The repeated appearance of the same wallet across multiple operational records strengthened attribution confidence and enabled correlation between:
- cryptocurrency activity
- exchange-related operational metadata
- user-associated infrastructure artifacts

During analysis inbound transaction `3b929f4cdb05a1455aa6dc28c5af02a4fa9c6c6120c4963897a2e1bc35546519` was established address attributed like `Bitzlato`-`1MEBYgciLY2irbijYLc4KkxDaGFHm3eyr4` transferred to our known address  `bc1qre3v2yfl4hr4f8uza66k7jlraxvphc6eaylk46`
![[phobos_deposit_transactions.png]]
Transaction scheme
![[phobos_related_wallet_address.png]]
Established account_id  allowed to find user id `13462376`
![[phobos_related_exchange_user_id.png]]
User info, including telegram ID:` 11******32`

Additional observation:

An inbound transaction (3b929f4c...) involving a Phobos-attributed wallet
was linked to a Bitzlato-associated address (1MEBYgci...).

This address was found in the wallet_addresses table and mapped to user_id `13462376`,
which was previously identified via withdrawal analysis.

This indicates repeated linkage of the same user across multiple transaction paths.

---
#### Key Insight

> The same wallet address is associated with multiple users.

---
#### 4.1 Cross-Transaction Correlation

Analysis revealed that the same user_id (`13462376`) appears in:

- Outbound transaction flows (withdrawal table)
- Inbound transaction flows (via wallet_addresses mapping)

This suggests:

- Consistent interaction of the user with funds originating from Phobos-attributed wallets
- Increased confidence in user involvement in the transaction flow

However:

Due to the absence of deposit data and the presence of pooled wallet architecture,
this does NOT confirm direct ownership of the wallet.

---
### 5. Identity Pivot: User Mapping

From `user_id`: `13461702`, `13462376`

Extracted:

- Telegram ID: `119*****77`, `119*****32`

---

### 6. OSINT Validation

Using **Wayback Machine**:

- Confirmed that exchange authentication relied on Telegram accounts

![[exchange_authentication_page.png]]

---
### Validation Outcome

> Telegram identifiers in the dataset are considered **reliable user-level identifiers**.

---
### 7. Telegram Attribution

- Resolved identities via Telegram OSINT
- Successfully identified: 1 out of 2 users**

Example:

- Telegram ID: 119*****77
- **Phone number:** `[REDACTED]`
- Username history analyzed:
	**23.03.2026** → `@Ni****227`, `11******77`
	**20.01.2026 →** `@TU*****ICK`, `[REDACTED]`, `11******77`
	**24.11.2025 →** `@TU*****ICK`, `11******77`
- Associated phone number identified

![[phobos_exchaner_user_identification.png]]

---

#### Limitation

> One Telegram identity remains unresolved (`119*****32`).

---

### 8. Advanced Finding: Exchange Wallet Architecture

The same wallet address was linked to multiple user_id values.  
Even when wallet-to-user linkage is observed across multiple transaction directions,
wallet reuse and internal routing mechanisms prevent reliable attribution of ownership.

This indicates that the identified wallets are not user-specific.
  
Impact:  
Direct attribution between wallet and user is unreliable.

---

## Interpretation

The exchange uses pooled wallet infrastructure.

This strongly indicates:

> These are **not user deposit wallets**, but internal exchange wallets:
- Aggregation wallets
- Hot wallets
- Internal treasury addresses

---

## Critical Insight

> Wallet reuse across multiple users confirms a **pooled wallet architecture**.

---

## Attribution Logic

Attribution is based on:

- Transaction → wallet linkage
- Wallet → user_id mapping
- User → Telegram identity
- External validation (Telegram-based authentication)

---

### Visualization
![[phobos_wallet_analysis_flowchart.png]]
- Cross-transaction user consistency (same user_id across independent flows)
---
## Bitzlato Wallet Analysis

Multiple Bitzlato-associated wallet addresses were identified via the wallet_addresses table.
Each wallet was mapped to a corresponding user_id.

However:
- No consistent overlap between users interacting with Phobos-attributed wallets was observed
- This further supports the presence of pooled wallet infrastructure

Implication:
Even when mapping wallet → user_id is technically possible, the absence of consistent overlap between users interacting with Phobos-attributed wallets confirms that wallet-level attribution
cannot be used as a reliable indicator of ownership.

Conclusion:
Wallet-level attribution to specific users is unreliable within this dataset.

## Limitations

- Deposit data missing
- Unable to establish full transaction path
- Wallet reuse violates direct attribution

---

## Risk Assessment

**Threat Level:** High

- Active ransomware-related financial flows
- Use of centralized exchange infrastructure
- Attribution ambiguity due to architecture

---

## Conclusion

While repeated linkage of user_id across multiple transaction paths increases confidence
in user involvement, the exchange’s pooled wallet architecture prevents definitive
wallet ownership attribution.

This distinction is critical to avoid false attribution.

This investigation demonstrates:

- Effective cross-layer analysis (DB + blockchain + OSINT)
- Partial attribution of ransomware-related actors
- Identification of structural limitations in exchange data

---

### Final Analytical Insight

> Understanding exchange architecture is critical to avoid false attribution in blockchain investigations.

---

## What This Case Demonstrates

- Database-driven threat intelligence analysis
- Blockchain attribution methodology
- OSINT-based identity resolution
- Critical evaluation of attribution reliability

---

## Investigation types

- [[ransomware]]
- [[leak_analysis]]

---
## Methodologies

- [[wallet_attribution]]
- [[behavioral_attribution]]

--- 
## Notes

- [[ransomware_wallet_patterns]]
- [[operational_security_failures]]