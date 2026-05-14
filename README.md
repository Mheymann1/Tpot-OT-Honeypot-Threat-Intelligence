# I Simulated an Oil & Gas Petroleum Tank Gauge on the Internet for 27 Days. Here's Who Showed Up.

> **1,000,000 attacks. 10+ nations. 2,000+ ICS/SCADA probes. A Cisco ASA honeypot hit 174,000 times.**  
> This is the real threat landscape facing O&G operators.

---

## The Setup

I aimed to address a fundamental inquiry: if a simulated Oil & Gas Operational Technology (OT) environment—designed to resemble authentic petroleum infrastructure—were placed on the internet, how soon would threat actors appear? Who would they be? What information would they seek?

Consequently, I established such an environment.

Utilizing Microsoft Azure, I deployed a T-Pot HIVE honeypot environment comprising over twenty simultaneous honeypots. The principal component was Conpot, configured with the Guardian AST template, which emulates a Veeder-Root automatic tank gauge—an identical device utilized for monitoring fuel inventories within petroleum storage facilities across the oil and gas sector.

Furthermore, I enhanced the security of the management plane by limiting Network Security Group (NSG) rules to my IP address, implementing SSH key-based authentication, encrypting disks via Azure Key Vault, and ensuring DISA STIG compliance through OpenSCAP. Conversely, I intentionally left the honeypot plane accessible without restrictions. This approach was deliberate.

Then I waited.

I did not have to wait for an extended period.

---

## Day One

Within hours of deployment, the scanners detected it. Not merely a few curious probes, but a continuous and unremitting series of connection attempts across all accessible ports. By the conclusion of the initial 24 hours, the tally of attacks was already escalating into the thousands.

This is the live attack map from that first day:

![Live Attack Map](screenshots/<img width="1706" height="691" alt="01_live_attack_map png  " src="https://github.com/user-attachments/assets/87928e0d-5a14-40cf-b7d5-d22e3adb2a17" />
)

**24,156 attacks in a single day.** Every line on that map represents a real connection attempt from a real IP address, spanning continents to probe my honeypot infrastructure.

I wasn't surprised that attacks came. I was surprised by how quickly, how automated, and how targeted they were.

---

## The Numbers After 27 Days

![Full Dashboard 1 Million](screenshots/<img width="1710" height="985" alt="02_full_dashboard_1m" src="https://github.com/user-attachments/assets/b3dd8a19-acb6-4683-84db-992f562bc2c8" />
)

After twenty-seven days, the totals exceeded a number I did not anticipate witnessing.

**One million attacks.**

| Honeypot | Attacks |
|---|---|
| Cowrie (SSH/Telnet) | 585,000 |
| Dionaea (Malware) | 313,000 |
| **Ciscoasa (VPN)** | **174,000** |
| Tanner (Web Apps) | 41,000 |
| H0neytr4p (Generic) | 34,000 |
| ElasticPot (Elasticsearch) | 25,000 |
| Adbhoney (Android/IoT) | 10,000 |
| Heralding (Credentials) | 7,000 |
| Mailoney (SMTP) | 6,000 |
| Honeytrap | 5,000 |
| **ConPot (ICS/SCADA)** | **2,000+** |
| **Total** | **1,000,000+** |

One million attacks were recorded against a single honeypot virtual machine over a period of twenty-seven days. Four significant figures emerge prominently from the data. Firstly, there were 585,000 SSH brute-force attempts facilitated through Cowrie, with the credentials employed revealing insights that will be discussed shortly. Secondly, 174,000 exploitation attempts targeting Cisco ASA VPNs were detected, a figure that bears implications for Oil & Gas operators that extend well beyond mere statistical representation. Lastly, over 2,000 probes aimed at ICS/SCADA systems were directed at the petroleum tank gauge. Although this number is relatively small compared to the others, these hits were specifically targeting industrial control system infrastructure, significantly altering the interpretation of this data.

---

## Origins and Geographic Distribution of Threats

![Country Breakdown](screenshots/<img width="435" height="288" alt="03_country_breakdown png " src="https://github.com/user-attachments/assets/eb557113-9399-4129-9316-6ba08f1af445" />
)

The prominence of the United States at the top of the list may initially surprise many. It was also unexpected to me at first. However, it is important to clarify the meaning behind this statistic: it does not indicate the presence of American threat actors per se. Rather, it signifies that compromised American infrastructure is being exploited as a launchpad for malicious activities. Sophisticated threat actors intentionally route their attacks through trusted nations in order to circumvent geopolitical IP restrictions. For instance, a threat actor utilizing a compromised server in Virginia would not be classified as originating from a foreign entity; instead, it appears as domestic traffic.

Similarly, France's presence among the top five countries illustrates the same pattern. This does not necessarily suggest threats originating from French threat actors; rather, it indicates that French infrastructure is being exploited as a pivot point.

The comprehensive breakdown by country is as follows:

🇺🇸 United States — Dominant source  
🇫🇷 France  
🇮🇳 India  
🇨🇳 China  
🇭🇰 Hong Kong  
🇮🇩 Indonesia  
🇧🇷 Brazil  
🇳🇱 Netherlands  
🇷🇺 Russia  
🇩🇪 Germany  

No continent is left unrepresented. The threat landscape is inherently global and distributed, functioning continuously around the clock.

It is important to note that oil and gas operators who rely solely on geopolitical IP blocking as a primary form of defense are addressing only a limited segment of the actual threat surface.

---

## Who These Attackers Actually Are

This finding stopped me.

![Attacker IP Reputation](screenshots/<img width="427" height="288" alt="04_ip_reputation png  " src="https://github.com/user-attachments/assets/a842d770-60ec-40fd-a9db-0c9d9ac8470e" />
)

The vast majority of attacking IP addresses had already been identified as known malicious actors within global threat intelligence feeds prior to their interaction with my honeypot. A smaller yet substantial portion were associated with anonymizers, VPNs, and proxy services intentionally concealing their origins.

These were not benign researchers nor accidental probes; rather, they were IPs with documented histories of malicious activity, actively continuing their attempts to target infrastructure.

Consider the implications of this for a reactive security posture. If your security approach is merely to "block IPs when they are flagged," you are already operating at a disadvantage. These IPs were flagged before, yet they persisted in their attacks.

Sole reliance on signature-based blocking does not constitute a comprehensive operational technology (OT) security strategy.

---

## The ICS/SCADA Narrative — Critical Considerations for the Oil & Gas Industry

This becomes especially pertinent for operators within the energy sector.

![Conpot ICS SCADA Dashboard](screenshots/<img width="1710" height="838" alt="05_conpot_ics_scada png  " src="https://github.com/user-attachments/assets/408da166-5534-47ef-a152-e9c6735189e8" />
)

The Conpot Guardian AST honeypot successfully detected over **2,000 targeted ICS/SCADA probes**, with each probe specifically directed at **port 161 (SNMP)**.

Unlike Modbus or S7comm, the focus was on SNMP.

This observation is noteworthy. Port 161 is exploited by malicious actors to identify operational technology (OT) devices prior to launching an attack. SNMP queries reveal vital information such as device type, firmware version, manufacturer, network topology, and connected assets. This activity constitutes reconnaissance — the preliminary phase preceding an attack.

Threat actors conducting scans for SNMP on petroleum infrastructure are not acting randomly. They are actively searching for specific devices such as Veeder-Root tank gauges, Emerson RTUs, and Honeywell flow computers. Their intent and targets are well defined.

The nations specifically engaging in targeting Conpot include the United States, the United Kingdom, Singapore, Canada, and Russia.

These five nations are persistently probing a simulated petroleum tank gauge via SNMP reconnaissance queries. The activity is sustained, deliberate, and consistently targets the same port — the reconnaissance port.

---

## The Cisco ASA Number — This Is How Colonial Pipeline Began

I repeatedly return to the fact that there were **174,000 attempts to exploit Cisco ASA systems**.

![Ciscoasa VPN Dashboard](screenshots/<img width="1710" height="803" alt="08_ciscoasa_vpn" src="https://github.com/user-attachments/assets/fbe89421-e0ff-4830-be96-ef74fe20cfcf" />
)

Cisco ASA VPN appliances are not merely generic networking devices. In the oil and gas sector, they are ubiquitous: pipeline control centers, offshore platform connectivity, remote wellhead monitoring, SCADA historian access, and contractor VPNs. They function as the primary access point to operational technology (OT) networks for remote connectivity.

The ransomware attack on Colonial Pipeline in May 2021 originated with the compromise of a single VPN credential on a legacy Cisco ASA profile. One credential was sufficient to halt fuel supplies to the U.S. East Coast for a period of six days.

Over a span of twenty-seven days, there were 174,000 attempts against simulated Cisco ASA infrastructure. Such activity is not incidental; it represents deliberate and persistent targeting of the specific technology safeguarding the perimeter of operational technology (OT) networks in the oil and gas industry. The nations targeting Cisco ASA systems specifically include the United States, Seychelles, Singapore, Russia, and Romania.

---

## What They Were Trying to Log In With

This is the part that should concern every OT operator.

![Credential Analysis](screenshots/<img width="1673" height="294" alt="06_credential_analysis png " src="https://github.com/user-attachments/assets/0347e979-6e60-418d-8f47-9f6849f43958" />
)

Cowrie recorded every username and password attempted during 585,000 SSH brute-force attacks. The following details demonstrate what attackers are currently utilizing:

**Top usernames:**
- `root` — dominant
- `admin`
- `ubuntu`
- `user`
- `dbadmin`
- `ftpuser`
- `AdminGPON` ← OT/router specific
- `orangepi` ← IoT/embedded device specific
- `oracle`
- `postgres`

**Top passwords:**
- **(blank)** ← the most common. Nothing. No password at all.
- `123456`
- `password`
- `admin`
- `root`
- `123456789`
- `!QAZ2wsx`
- `P@ssw0rd`

The most commonly attempted password among 585,000 brute force attempts was a blank field. This is no coincidence. Attackers frequently utilize blank passwords because they are effective — particularly in operational technology (OT) environments, where Programmable Logic Controllers (PLCs), Remote Terminal Units (RTUs), Human-Machine Interfaces (HMIs), and tank monitoring systems often ship without a default password and are seldom configured with one. These devices remain on operational networks for years, sometimes even decades, with their factory defaults unaltered.

The same credential list that is currently targeting my SSH honeypot is also being used to access exposed OT assets within energy infrastructure at this moment.

---

## The Global Picture

![Dynamic Attack Map](screenshots/<img width="576" height="284" alt="07_dynamic_attack_map png " src="https://github.com/user-attachments/assets/a46b3e9d-9def-4a55-8e30-065a7b2bd4e1" />
)

Each bubble on this map signifies a source of attacks directed at simulated petroleum infrastructure over a period of twenty-seven days. The size of each bubble correlates with the volume of attacks.

North America. Europe. Asia. South America. Africa. Oceania.

No continent is exempt; no region is secure. The attack surface is not confined to specific regions — it is planetary in scope.

---

## What This Maps To — MITRE ATT&CK

**ICS ATT&CK:**

| Tactic | Technique | Evidence |
|---|---|---|
| Reconnaissance | T0840 — Network Service Scanning | SNMP probes against Conpot |
| Initial Access | T0866 — Exploitation of Remote Services | Ciscoasa VPN exploitation |
| Initial Access | T0886 — Remote Services | SSH brute force |
| Credential Access | T0812 — Default Credentials | Blank passwords + default spray |
| Discovery | T0846 — Remote System Discovery | SNMP enumeration |
| Collection | T0802 — Automated Collection | Automated scanning infrastructure |

**Enterprise ATT&CK:**

| Tactic | Technique | Evidence |
|---|---|---|
| Reconnaissance | T1595 — Active Scanning | Mass port scanning |
| Initial Access | T1190 — Exploit Public-Facing Application | Ciscoasa, ElasticPot |
| Initial Access | T1133 — External Remote Services | VPN exploitation |
| Credential Access | T1110 — Brute Force | 585,000 SSH attempts |
| Credential Access | T1078 — Valid Accounts | Default credential spraying |
| Discovery | T1046 — Network Service Discovery | SNMP enumeration |

---

## NERC CIP Alignment

| Standard | Requirement | Honeypot Evidence |
|---|---|---|
| **CIP-005** | Electronic Security Perimeters | VPN exploitation demonstrates perimeter risk |
| **CIP-007** | System Security Management | Default credentials, unpatched VPN vulnerabilities |
| **CIP-010** | Configuration Change Management | Default credential persistence |
| **CIP-011** | Information Protection | SNMP data enumeration attempts |
| **CIP-013** | Supply Chain Risk Management | OT device default credential risk |

---

## Guidance for Oil & Gas Operators — Immediate and Short-Term Security Measures

### Immediate Actions

- **Change all default credentials** on every operational technology (OT) device, including Programmable Logic Controllers (PLCs), Remote Terminal Units (RTUs), Human-Machine Interfaces (HMIs), tank gauges, and flow computers. Specifically, replace any credentials such as `admin/admin` or any blank passwords.
- **Disable SNMP versions 1 and 2c**, and migrate to SNMP version 3, utilizing authentication and encryption. If SNMP is not necessary for operational purposes, disable it entirely.
- **Ensure your Cisco ASA VPN firmware is patched** — approximately 174,000 attempts persistently occur and known CVEs on unpatched ASA devices are actively exploited. Confirm the current firmware version.
- **Implement Multi-Factor Authentication (MFA)** for all VPN access points. A single compromised credential should not permit network access, as demonstrated by the Colonial Pipeline incident.

### Short-Term Strategies (Within 90 Days)

- **Deploy OT-specific monitoring solutions** such as Claroty, Dragos, or Nozomi Networks to enable behavioral detection capabilities that signature-based tools may overlook.
- **Conduct a comprehensive inventory of OT assets.** Protecting assets without awareness of their existence is ineffective; identify every internet-connected OT device within your environment.
- **Review firewall rules** to determine if any OT protocols — such as Modbus, S7comm, ENIP, or BACnet — are exposed directly to the internet. These protocols should not be accessible from the public internet.

---

## The Key Findings

This honeypot was active for a period of twenty-seven days, during which it encountered one million malicious attacks. The most frequently targeted piece of infrastructure was a simulated Cisco ASA VPN. The predominant OT protocol subject to reconnaissance was SNMP, with no exploitation yet observed. The most commonly used password was nothing.

These observations are based on actual attacks originating from genuine IP addresses against simulated petroleum infrastructure hosted on Azure.

For those in the oil and gas sector, the pertinent question is not whether their infrastructure is being targeted — this data provides that answer.

**The critical question is whether they would be aware if an intrusion had occurred.**

---

## Technical Stack

| Tool | Purpose |
|---|---|
| T-Pot HIVE | Multi-honeypot platform |
| Microsoft Azure | Cloud infrastructure |
| Conpot Guardian AST | Petroleum ATG simulation |
| Elasticsearch + Kibana | Data storage and visualization |
| Logstash | Log aggregation |
| Suricata | IDS alerting |
| SpiderFoot | Attacker IP intelligence |
| Azure NSG | Network access control |
| Azure Key Vault | Disk encryption |
| OpenSCAP | DISA STIG compliance |
| Azure Defender for Cloud | Security posture management |

---

## Screenshots

| File | Description |
|---|---|
| `01_live_attack_map.png` | Live T-Pot attack map — 24,156 attacks in 24 hours |
| `02_full_dashboard_1m.png` | Full dashboard — 1,000,000 total attacks |
| `03_country_breakdown.png` | Attacks by country pie chart |
| `04_ip_reputation.png` | Attacker source IP reputation breakdown |
| `05_conpot_ics_scada.png` | Conpot ICS/SCADA full dashboard — port 161 analysis |
| `06_credential_analysis.png` | Username and password tag clouds |
| `07_dynamic_attack_map.png` | Global bubble attack map |
| `08_ciscoasa_vpn.png` | Ciscoasa VPN exploitation full dashboard |

---

## About

I came to cybersecurity through the field — not a classroom.

Five years working within pharmaceutical manufacturing OT environments gave me a firsthand understanding of what it means when industrial systems fail, when processes go offline, and when the gap between IT and OT becomes a liability. Another five years operating in CBRN environments taught me how to assess risk under pressure, follow strict protocols, and understand the consequences when safety systems don't work as intended.

That background shapes how I approach OT security. I don't see it as an abstract compliance exercise. I see it as protecting the systems that keep real operations running.

Today I work as a Contributing Security Analyst with hands-on Splunk Enterprise Security expertise, pursuing an MS in Cybersecurity and Information Assurance. This honeypot project was built to answer a question I kept asking in the field — what does the real threat landscape targeting OT infrastructure actually look like?

Now I have data.

**Certifications:** CySA+ | CSOCA | CEH

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/mheymann1)
[![GitHub](https://img.shields.io/badge/GitHub-mheymann1-black)](https://github.com/mheymann1)

---

*All data collected from attacks against infrastructure owned and operated by the author. No real OT systems were involved. Published for educational and research purposes.*
