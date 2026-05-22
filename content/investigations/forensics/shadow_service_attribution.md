---
title: Shadow Marketplace Attribution

tags:
  - darknet
  - underground-marketplace
  - cryptocurrency
  - forensic-analysis
  - osint
  - attribution
  - php
  - database-analysis
  - blockchain

type: investigation
status: completed
confidence: high

investigation-type:
  - infrastructure-investigations
  - threat-actor-attribution
  - leak-analysis

methodologies:
  - infrastructure-artifact-analysis
  - behavioral-attribution
  - osint-deanonymization
  - wallet-attribution

tools:
  - autopsy
  - mysql
  - php
  - python
  - maxmind
  - trm-labs
---
> Infrastructure-focused investigation of a long-running shadow service involving forensic image analysis, database decryption, OSINT correlation, and cryptocurrency attribution.

---

## Executive Summary

An investigation was initiated to identify individuals potentially involved in administration and operational support of the shadow marketplace `ba****ls.al`, a long-running underground service active since at least 2017.

The platform specialized in the sale of:
- social media accounts
- dating platform accounts
- RDP access
- SMS verification services
- compromised “Big Tech” accounts
- security-related services

Initial attribution efforts produced only low-confidence regional indicators associated with the Balkan region.

The investigation significantly advanced following acquisition of:
- a multi-table encrypted SQL database
- a full forensic image of the service infrastructure (~975 GB unpacked)
- thousands of operational records and IP artifacts

Subsequent forensic analysis enabled:
- identification of encryption mechanisms
- recovery of encryption/decryption routines
- full database decryption
- user segmentation and geo-enrichment
- cryptocurrency attribution linked to platform activity

The investigation resulted in identification of cryptocurrency infrastructure not previously attributed to the service in blockchain intelligence platforms.

The case demonstrated how forensic exploitation of infrastructure artifacts can overcome otherwise inaccessible encrypted operational datasets.

---

## My Role

- Led infrastructure-focused investigation
- Performed SQL database analysis
- Conducted forensic artifact analysis
- Identified encryption implementation
- Recovered encryption/decryption logic
- Developed automation workflows for data processing
- Conducted geo-enrichment and user segmentation
- Performed cryptocurrency attribution and enrichment
- Prepared investigative findings for LE handoff

> Focus: combining forensic analysis, OSINT, and blockchain attribution to identify operational infrastructure and associated entities.

---

## Investigation Context

The investigation focused on `bas*****.al`, a shadow-oriented service operating since approximately 2017.

The platform advertised:
- account sales
- RDP access
- SMS verification services
- compromised accounts
- security-related tooling

The platform operated as a multi-service underground marketplace supporting account sales, access services, and operational tooling frequently associated with fraud ecosystems and account abuse operations.
The service appeared to operate as a long-term operational marketplace rather than a short-lived opportunistic platform.

![[shadow_marketplace_main_page.png]]
Screenshot is shown main source structure, in the left sidebar we see main services list 

---

##  Initial Attribution Attempts

Initial monitoring through threat intelligence platforms identified:
- the identified email accounts appeared repeatedly across promotional activity and service-related operational artifacts.
- possible multi-account activity used to promote the service

However, attribution efforts stalled at:
- low-confidence Balkan-region associations
- probable Serbian infrastructure indicators

No direct operational attribution was established during this phase.
At this stage, available indicators were insufficient to establish high-confidence attribution or identify core operators.

> Analyst Note:  
> Early-stage attribution produced only weak regional indicators insufficient for operational confidence.

![[shadow_marketplace_infrastructure_attribution.png]]
[View Full Resolution Graph](graphs/base_marketplace_admin_investigation.png)

Source domain investigation process scheme.

>Sensitive data, operational identifiers, and personally identifiable information have been partially redacted for legal and ethical reasons.

---

## Database Acquisition & Analysis

The investigation significantly advanced following acquisition of a SQL database dump associated with the platform infrastructure.
The database was restored locally for analysis.
During review of database tables, several operationally significant fields were identified as encrypted.

![[shadow_marketplace_encrypted_users_table.png]]

Encrypted fields were identified across multiple operational tables, including:
- email records
- cryptocurrency-related fields
- account metadata
- user-associated operational data
---

## Encryption Analysis

Analysis of several operational database fields revealed values with characteristics inconsistent with standard plaintext application data.

The observed values did not match characteristics typically associated with one-way password hashing, as identical structural patterns and successful reversible decoding logic were later identified.

Examples included:
- high-entropy strings
- fixed-length encoded values
- repeated structural patterns across records
- Base64-like formatting

Notably, fields typically expected to contain readable operational data — including email records — consistently exhibited encoded high-entropy structures.

![[shadow_marketplace_encryption_identification.png]]
Example email values:

`3XPsPhp50XTfy3Qx7muzUq3IFIYxDcWCh9Hvuk1t2d4=`
`5/X0sUIls9fpb40QWJCNEBITljd0lzB0npAjSgEVMfc=`
`7rZzuSV61xo5Jh9+bLghsAgBPbmzZYeIs/noNMHQES0=`

The consistency of these characteristics across multiple tables strongly suggested the presence of application-level encryption rather than random corruption or hashing artifacts.

Further analysis indicated:
- repeatable encryption structure
- centralized encryption implementation
- custom encryption logic embedded within the application architecture

Automated brute-force analysis was assessed as operationally inefficient due to:
- unknown key structure
- implementation complexity
- lack of algorithm visibility at the time
- time constraints

At this stage, direct analysis of operational records remained blocked.

---

## Forensic Image Exploitation

A subsequent forensic image of the platform infrastructure was obtained.

### Image Characteristics:
- ~975 GB unpacked size
- full server image
- web application artifacts
- database-related operational files

The forensic image was processed using:
- Autopsy
- manual artifact analysis
- targeted filesystem review

Priority was given to application-layer artifacts likely to contain operational logic, configuration data, or cryptographic implementations.

Due to the image size, full indexing timelines were operationally impractical.

Key Investigative Breakthrough

A decision was made to prioritize:
- manual artifact discovery
- targeted analysis of application directories
- review of potentially sensitive operational scripts

> Analyst Note:  
> Manual artifact analysis proved significantly more efficient than waiting for full forensic indexing of the server image.


![[shadow_marketplace_autopsy.png]]
[View Full Resolution Graph](shadow_marketplace_image_autopsy_analysis.png)
Autopsy observation process scheme.

---

## Key Investigative Breakthrough

The investigation pivoted after identification of a PHP application script containing the encryption and decryption routines used by the platform.

Manual analysis of the web application directory revealed:
- encryption implementation
- decryption workflow
- associated cryptographic key material

This discovery enabled operational access to previously unreadable database records.

```php
<?php

class Encryption {
    private $skey = "[REDACTED]";

    public function decode($value){
        $crypttext = base64_decode($value); 
        $iv_size = mcrypt_get_iv_size(MCRYPT_RIJNDAEL_256, MCRYPT_MODE_ECB);
        $iv = mcrypt_create_iv($iv_size, MCRYPT_RAND);
        $decrypttext = mcrypt_decrypt(MCRYPT_RIJNDAEL_256, $this->skey, $crypttext, MCRYPT_MODE_ECB, $iv);
        return trim($decrypttext);
    }
}

$enc = new Encryption();

echo $enc->decode($argv[1]) . PHP_EOL;
```

The recovered implementation confirmed that the platform relied on application-layer symmetric encryption embedded directly within operational PHP components.

Recovery of application-layer cryptographic routines transformed the investigation from artifact collection into direct operational data analysis.

---

## Database Decryption Workflow

Following recovery of the encryption logic, custom automation scripts were developed to:
- validate partial decryption
- test field integrity
- perform full database decryption
- export readable operational records

The workflow was initially tested against selected records before scaling to complete table processing.

![[shadow_market_encryption_flow.png]]
Successful validation enabled large-scale decryption of operational records across multiple database tables.

Decrypted records included operational user data, infrastructure-related metadata, and cryptocurrency-associated references previously inaccessible during earlier investigation stages.

---

## User Segmentation & Geo-Enrichment

Following successful database decryption, operational records became available for structured analysis.

All IP addresses were extracted and enriched using:
- MaxMind GeoIP datasets
- custom automation workflows
- targeted regional filtering

Subsequent analysis prioritized Ukrainian-linked activity based on investigative tasking and emerging operational correlations identified within decrypted records.
### Geo-Enrichment Workflow

![[shadow_marketplace_geo_analysis.png]]
### Automation Approach

Custom scripts were developed to automate:
- IP extraction
- GeoIP enrichment
- regional filtering
- export of segmented operational records

These workflows enabled rapid prioritization of operationally relevant datasets that would otherwise require extensive manual review.

Example logic used for regional filtering:

```python
response = reader.country(ip)

if response.country.iso_code == "UA":
    outfile.write(ip + "\n")
```

The resulting dataset enabled prioritization of Ukrainian-linked operational activity and subsequent correlation with cryptocurrency-related records.
Custom automation workflows were used to process and segment operational records at scale.

> Analyst Note:
> Automation workflows significantly reduced manual review time and enabled targeted analysis of operationally relevant user segments.

---
## Cryptocurrency Attribution

Correlation of decrypted user records with cryptocurrency wallet tables enabled attribution of a cryptocurrency address associated with platform activity.

The identified address:
- was not previously attributed to the service in TRM Labs
- demonstrated operational linkage to identified user records
- was subsequently enriched using internal investigative platforms

Additional enrichment included:
- transaction exposure analysis
- related account discovery
- infrastructure correlation

The resulting attribution chain linked decrypted operational records with blockchain activity through shared user-associated artifacts.
![[shadow_market_user_deanon.png]]
[View Full Resolution Graph](base_marketplace_user_attribution_graph.png)

Correlation was established through overlap between decrypted user-associated records and cryptocurrency wallet references contained within operational database tables.

> Analyst Note:  
> The identified cryptocurrency infrastructure had not previously been associated with the platform in commercial blockchain intelligence tooling.

---

## Operational Constraints

### Key Challenges  
  
- The forensic server image exceeded ~975 GB unpacked size, resulting in impractical indexing timelines for traditional full-scale forensic processing.  
- Multiple operational database fields were protected through application-layer encryption, preventing immediate visibility into user-associated records.  
- Initial attribution efforts produced only low-confidence regional indicators with limited operational value.  
- Operational artifacts were fragmented across:  
- database records  
- application files  
- infrastructure components  
- user-associated metadata  

No single artifact independently provided sufficient attribution confidence prior to database decryption.
### Investigative Tradeoffs  
  
Traditional exhaustive forensic workflows were deprioritized in favor of targeted artifact discovery and operationally focused analysis.  
  
Key investigative decisions included:  
- prioritizing manual review of application-layer artifacts  
- focusing on potentially sensitive web application files  
- using selective enrichment workflows instead of large-scale brute-force analysis  
- validating partial decryption workflows before scaling to complete database processing

---

##  Investigative Outcome

The investigation successfully:
- identified encryption implementation used by the platform
- recovered operational decryption logic
- enabled full database visibility
- segmented operational users by geography
- identified cryptocurrency infrastructure linked to platform activity
- produced investigative findings suitable for LE escalation

The investigation transformed previously inaccessible encrypted operational data into actionable attribution intelligence.

The resulting intelligence package was transferred for additional legal and operational verification.

---
## Impact

- Enabled operational visibility into encrypted marketplace data
- Identified previously unattributed cryptocurrency infrastructure
- Demonstrated linkage between platform users and crypto activity
- Expanded attribution coverage beyond commercial blockchain intelligence datasets
- The investigation produced previously unavailable attribution intelligence suitable for escalation and additional operational verification

These decisions significantly reduced analysis timelines and enabled direct access to operational records that would otherwise have remained unavailable through conventional indexing workflows alone.  

The investigation demonstrated how infrastructure-level forensic exploitation can directly expand blockchain attribution coverage beyond commercially available intelligence datasets.
  
> Analyst Note:  
> Operational efficiency was prioritized over exhaustive processing in order to accelerate attribution-focused investigative outcomes.

---

##  Key Takeaways

- Application-layer artifacts can provide critical investigative pivots
- Manual forensic analysis may outperform full indexing workflows in large-scale investigations
- Encrypted operational databases often become accessible through infrastructure artifact analysis
- Combining forensic exploitation with blockchain analysis significantly improves attribution outcomes

---

## Tools & Skills Demonstrated

### Forensics

- Autopsy
- filesystem artifact analysis
- infrastructure review

### Blockchain Investigation

- TRM Labs
- wallet attribution
- attribution enrichment  
- operational wallet correlation

### OSINT & Enrichment

- GeoIP enrichment
- infrastructure correlation
- identity linkage

### Technical

- SQL analysis
- encryption analysis
- automation scripting
- data extraction and transformation
- database restoration
- structured record analysis
### Application Analysis  

- PHP application review  
- encryption routine analysis  
- operational artifact discovery

---
## Methodologies  
  
- [[application_layer_encryption_analysis]]
- [[geoip_user_segmentation]]
- [[infrastructure_artifact_analysis]]

---
## Investigation Types

- [[underground_marketplaces]]
- [[infrastructure_investigations]]
- [[leak_analysis]]

---
## Notes

- [[operational_security_failures]]