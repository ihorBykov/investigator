---
title: Pig Butchering Scam Investigation
tags:
  - fraud
  - pig-butchering
  - cryptocurrency
  - laundering
  - tron
  - social-engineering
  - blockchain
type: investigation
status: completed
confidence: high
investigation-type:
  - fraud-investigations
  - blockchain-investigations
methodologies:
  - wallet-attribution
  - behavioral-attribution
related-notes:
  - tron-laundering-observations
tools:
  - trm-labs
  - metasleuth
  - python
---
## 1. Executive Summary

During the analysis of a report from Chainabuse, a Pig Butchering / Fake Return Scam scheme was identified, utilizing a multi-chain infrastructure (TRON and Bitcoin).

A TRON address was used for initial fund collection (USDT), after which funds were distributed through a fan-out structure with partial deposits to centralized exchanges, including Binance, OKX, and Bybit.

In the Bitcoin network, a classical peel chain pattern was observed, with repeated structured withdrawals to Bitget, indicating a controlled cash-out phase.

The domain infrastructure is disposable and does not provide attribution opportunities.

The overall behavior suggests a hybrid laundering model separating distribution and cash-out stages across different blockchains. 

---
## 2. Source Information

**Source:** Chainabuse  
**Type:** User-submitted fraud report  
**Category:** Pig Butchering Scams/Fake Return Scams: Secure Your Crypto Investments
**Report date:** 20.03.2026
**Report link:** `https://chainabuse.com/report/b4c599eb-55fe-4a29-a962-9cf7d13c6a78?context=search-address&a=TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY&chain=`

---
## 3. Initial Artifacts

### 3.1 Confirmed Artifacts
- TRON address: `
	- TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY` (since 2023-08-22)
- Domains:
	- ZYOET.COM
	- MXCOIN.COM

---

### 3.2 Derived Artifacts

- additional domain names:
	- PRIMECOIN.COM
	- LYQET.COM
	- LYQET.COM-HELPED.ME
	- RECOVER-MY.MONEY
	- VISIT-LYQET.COM-FOR.HELP
- TRON addresses fan-out chain:
    `TLQsdQZwcBAmAc3AFAPE3yR2iF1ehZkNJt`
    `TMXZAySpsog7WtaeKb8WotoHepNsD1jjTc`
    `TU3UL83ELDMhDEMUXgA9EsuPHamSaKt46H`
    `TV4Jy4HqU9BysMxj3LNWVAaCy1sM9pb6S5`
- VASP:
	- ByBit Deposit:
		- `THc8RXUGgvvrMReQaQiF9Z12wwUHgJsQwE`
	- Binance Deposit:
		- `TZ8GJfaPeuB71ksXWcBRL5FGa3F7WreskJ`
	- OKX Deposit:
		- `TCY7gDgi3xP446FF4pbTcXX6y4CfKozNRC`
		- `TDWaQ8C3umiohBCdrSSXp9gUwJ6AHd3td5`
		- `TEVuakePh3uQzXL3xyQoiEamVsrQ7TAvEz`

- additional BTC address:
	- '1CuR2yw1dizU3MfNfrBSeyZ3tuK9uRxGrL'
- VASP:
	- Bitget Deposit:
		- `15AEVy4PzmrphJ9KRRqcAv3BbwBDT6vR69`
## 4. Infrastructure Analysis (Off-chain)

### 4.1 Domain Characteristics

Identified domains exhibit characteristics typical of short-lived scam infrastructure:
- No presence in web archives
- Currently inactive or suspended
- Registered shortly before the incident
- No reuse observed
Domain registrars include Namecheap and Hostinger.

These indicators suggest that the infrastructure was created specifically for a short-term fraudulent campaign. (ZYOET.COM created Feb 13, 2026 4:42 PM).

### 4.2 Behavioral Assessment

**Infrastructure:**
- **Disposable:** This is evident from the initial observation that the registration date was only a few days prior to the incident.
- **Non-reusable:** Following the formal abuse report, the domain has been flagged as malicious; currently, resolving the domain leads to the `therapyexpertrecovery.com` landing page.

**Conclusion:**

> The domain infrastructure was provisioned exclusively for a **short-lived fraudulent campaign**.

---
### 4.3 WHOIS Analysis (False Lead)

A phone number identified in WHOIS records was initially considered as a pivot point.

However, further validation showed that the number belongs to the domain registrar Namecheap and is reused across unrelated domain registrations.

**Check results:**
- phone number: `+3544212434`
- domain registrant: `namecheap.com`

**Conclusion:**

>This artifact is therefore classified as infrastructure noise and not linked to the threat actor.

---
## 5. On-chain Analysis

### 5.1 TRON / USDT Transaction Pattern

Figure 1. TRON fan-out transaction graph illustrating distribution of funds across multiple wallets with partial deposits to CEXs.
![[tron_pig_butchering.svg|697]]

### Pattern:
- A fan-out (tree-like) transaction pattern was identified in the TRON network.
### Behavior:
- exponential distribution of funds across multiple wallets;
- partial deposits to centralized exchanges:
	- Binance
	- OKX
	- Bybit
- Continued redistribution across secondary addresses
### Interpretation:
> Funds are fragmented across multiple wallets with partial CEX interaction, indicating mixed obfuscation and early-stage cash-out.

---
### 5.2 Bitcoin Transaction Pattern

Figure 2. BTC peel chain transaction graph illustrating automated transaction execution with partial deposits to Bitget.
![[peelchain_transaction_templates.png]]
### Pattern:
- peel chain
### Behavior:
- repeated fixed-size withdrawals
- sequential balance reduction
- high-frequency transactions with overlapping timestamps
### Key Finding:
- regular withdrawal to Bitget
### Interpretation:
> Repeated fixed-size withdrawals suggest automated transaction execution

---
## 6. Behavioral Pattern (TTP Analysis)

The following TTPs were identified:
- Use of disposable domain infrastructure
- Multi-chain laundering strategy
- TRON used for distribution and obfuscation
- Bitcoin used for structured cash-out
- Peel chain (linear obfuscation)
- Fan-out (exponential obfuscation)

**Conclusion:**
This combination indicates a hybrid laundering model designed to separate distribution and cash-out phases.

---
## 7. Linked Reports

**Additional Chainabuse reports identified.**

- **Cross-referenced Chainabuse reports for the specific wallet address** 'TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY'
	- ` https://chainabuse.com/report/245f9ff2-1463-4966-aa7b-86c6ac6167c9?context=search-address&a=TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY&chain=`
	- `https://chainabuse.com/report/b4c599eb-55fe-4a29-a962-9cf7d13c6a78?context=search-address&a=TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY&chain=`
	- `https://chainabuse.com/report/b6873542-68e6-4c3b-a7ff-a40721ef55f2?context=search-address&a=TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY&chain=`
	-  `https://chainabuse.com/report/fe326fb8-6b0f-45e0-83d1-ebc684a62b0e?context=search-address&a=TKpZjLExcW3FVPnNrR51oGwdpN3V3sUGtY&chain=`
- **Linked Chainabuse reports associated with the domain  'PRIMECOIN.COM'**
	- ` https://chainabuse.com/report/6273f0e1-3511-42e3-8681-ab5f65cd13c6?context=search-domain&d=`
- **Linked Chainabuse reports associated with the domain 'LYQET.COM'.**
	- `https://chainabuse.com/report/6273f0e1-3511-42e3-8681-ab5f65cd13c6`

****Connections:****
- **Cryptocurrency address overlap**;
- **Use of identical domain infrastructure**

---
## 8. Attribution Assessment

No off-chain attribution was identified.
The infrastructure does not provide reusable identifiers, and no identity linkage was established.

However, behavioral attribution is possible based on consistent transaction patterns.

**Confidence level:**
- Identity attribution: Low
- Behavioral attribution: Medium
---

## 9. Intelligence Value

### Detection Opportunities
- Peel chain patterns with repeated CEX deposits
- TRON fan-out bursts
### Monitoring
- short-lived domains
- lack of WHOIS attribution
### Use Cases
- AML / compliance
- fraud detection
- blockchain analytics

---
## 10. Recommendations

### For Bitget
- detect repeated structured deposits from peel chains
- correlate deposits with upstream fan-out activity
### For Analysts
- track fan-out trees
- identify repeated patterns

---
## 11. Key Takeaways
- **The lack of off-chain data does not diminish the case's analytical value.**
- **Behavioral analysis enables the identification of TTPs (Tactics, Techniques, and Procedures).**
- **Hybrid schemes are becoming the industry standard in crypto fraud.**

---
## Methodologies

- [[wallet_attribution]]
- [[behavioral_attribution]]

---
## Investigation Types

- [[pig_butchering]]
- [[fraud_investigations]]
- [[blockchain_investigations]]

---
## Notes

- [[tron_laundering_observations]]