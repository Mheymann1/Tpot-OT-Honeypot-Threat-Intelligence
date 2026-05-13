# I Simulated an Oil & Gas Petroleum Tank Gauge on the Internet for 40 Days. Here's Who Showed Up.

> **1,000,000 attacks. 10+ nations. 2,000+ ICS/SCADA probes. A Cisco ASA honeypot hit 174,000 times.**  
> This is what the real threat landscape facing O&G operators looks like.

---

## The Setup

I wanted to answer a simple question.

If you put a simulated Oil & Gas OT environment on the internet — something that looks and behaves like real petroleum infrastructure — how long before threat actors show up? Who are they? What are they looking for?

So I built one.

On Microsoft Azure, I deployed a T-Pot HIVE honeypot environment running 20+ simultaneous honeypots. The crown jewel was **Conpot with the Guardian AST template** — a simulation of a Veeder-Root automatic tank gauge, the exact type of device used to monitor fuel inventory in petroleum storage facilities across the O&G industry.

I hardened the management plane — NSG rules locked to my IP only, SSH key authentication, disk encryption via Azure Key Vault, DISA STIG compliance via OpenSCAP. The honeypot plane I left wide open, intentionally. That's the point.

Then I waited.

I didn't have to wait long.

---

## Day One

Within hours of deployment, the scanners found it.

Not one or two curious probes — a constant, relentless stream of connection attempts across every exposed port. By the end of the first 24 hours, the attack counter was already climbing into the thousands.

This is the live attack map from that first day:

![Live Attack Map](screenshots/<img width="1706" height="691" alt="01_live_attack_map png  " src="https://github.com/user-attachments/assets/51818145-0566-48e1-9c73-58111badb3dc" />
)

**24,156 attacks in a single day.** Every line on that map is a real connection attempt from a real IP address, reaching across continents to probe my honeypot infrastructure.

I wasn't surprised that attacks came. I was surprised by how fast, how automated and how targeted they were.

---

## The Numbers After 40 Days

![Full Dashboard 1 Million](screenshots/<img width="1710" height="985" alt="02_full_dashboard_1m" src="https://github.com/user-attachments/assets/0279c3ef-21cb-4588-957b-8227990eef82" />


After forty days, the totals crossed a number I didn't expect to see:

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

One million attacks. Against a single honeypot VM. In forty days.

Three numbers stand out above all others.

**585,000 SSH brute force attempts** through Cowrie. The credentials being used told a story I'll get to shortly.

**174,000 Cisco ASA VPN exploitation attempts.** That number has implications specifically for O&G operators that go far beyond a statistics table.

**2,000+ ICS/SCADA probes** against the petroleum tank gauge. Small number compared to the others. But those 2,000 hits were specifically targeting industrial control system infrastructure. That changes everything about how you interpret them.

---

## Where They Came From

Here's the geographic breakdown:

![Country Breakdown](screenshots/<img width="435" height="288" alt="03_country_breakdown png " src="https://github.com/user-attachments/assets/8b6906b3-0026-4f09-9cbb-b095b40d9edf" />
)

The United States sitting at the top surprises people. It surprised me too, at first.

But here's what that number actually means — **it doesn't mean American threat actors**. It means compromised American infrastructure being weaponized as a launchpad. Sophisticated actors deliberately route attacks through trusted nations to evade geopolitical IP blocking. A threat actor running attacks through a compromised server in Virginia doesn't show up as foreign traffic. It shows up as American.

France appearing prominently in the top five tells the same story. Not French threat actors — French infrastructure being exploited as a pivot point.

The full country breakdown:

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

No continent was absent. The threat is global, distributed and it runs 24 hours a day without pause.

O&G operators who implement geopolitical IP blocking as a primary defense are addressing a fraction of the actual threat surface.

---

## Who These Attackers Actually Are

This is the finding that stopped me.

![Attacker IP Reputation](screenshots/<img width="427" height="288" alt="04_ip_reputation png  " src="https://github.com/user-attachments/assets/a27e6866-02fe-4aee-b0cc-3d9de250f8dd" />
)

The overwhelming majority of attacking IPs were already **flagged as known attackers** in global threat intelligence feeds before they ever hit my honeypot. A smaller but significant slice were anonymizers — VPNs and proxy services deliberately masking origin.

These weren't curious researchers. They weren't accidental probes. They were IPs with existing malicious track records, already documented, still actively targeting infrastructure.

Think about what that means for a reactive security posture. If your defense strategy is "we'll block IPs when they get flagged" — you're already behind. These IPs were already flagged. They kept attacking anyway.

Signature-based blocking alone is not an OT security strategy.

---

## The ICS/SCADA Story — What Really Matters for O&G

This is where it gets specifically relevant to energy sector operators.

![Conpot ICS SCADA Dashboard](screenshots/<img width="1710" height="838" alt="05_conpot_ics_scada png  " src="https://github.com/user-attachments/assets/8d3db6fd-8788-4540-a4b8-eb3b0470616e" />
)

The Conpot Guardian AST honeypot received **2,000+ targeted ICS/SCADA probes** — and every single one targeted **port 161 (SNMP)**.

Not Modbus. Not S7comm. SNMP.

That's significant. Port 161 is how attackers **enumerate OT devices** before they attack them. SNMP queries return device type, firmware version, manufacturer, network topology, connected assets. It's reconnaissance — the step before the actual attack.

Threat actors scanning SNMP on petroleum infrastructure aren't randomly stumbling onto it. They're looking for Veeder-Root tank gauges, Emerson RTUs, Honeywell flow computers. They know what they're hunting.

The attacking nations specifically targeting Conpot: United States, United Kingdom, Singapore, Canada and Russia.

Five nations probing a simulated petroleum tank gauge with SNMP reconnaissance queries. Sustained. Deliberate. Every single probe targeting the same port — the reconnaissance port.

---

## The Cisco ASA Number — This Is How Colonial Pipeline Started

I keep coming back to **174,000 Ciscoasa exploitation attempts**.

![Ciscoasa VPN Dashboard](screenshots/<img width="1710" height="803" alt="08_ciscoasa_vpn" src="https://github.com/user-attachments/assets/27009146-b1d2-498f-836d-4daff12063d5" />
)

Cisco ASA VPN appliances aren't just generic network gear. In O&G they're everywhere — pipeline control centers, offshore platform connectivity, remote wellhead monitoring, SCADA historian access, contractor VPN. They are the front door to OT networks for remote access.

The Colonial Pipeline ransomware attack in May 2021 started with a single compromised VPN credential on a legacy Cisco ASA profile. One credential. That's all it took to shut down fuel supply to the US East Coast for six days.

174,000 attempts against simulated Cisco ASA infrastructure in forty days is not background noise. That is deliberate, sustained targeting of the exact technology protecting O&G OT network perimeters. The attacking nations on Ciscoasa specifically: United States, Seychelles, Singapore, Russia and Romania.

---

## What They Were Trying to Log In With

This is the part that should concern every OT operator.

![Credential Analysis](screenshots/<img width="1673" height="294" alt="06_credential_analysis png " src="https://github.com/user-attachments/assets/55055be5-bd5c-44bb-a200-ba630b6f3b51" />
)

Cowrie captured every username and password attempted across 585,000 SSH brute force attempts. Here's what attackers are actually using:

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

The most attempted password across 585,000 brute force attempts was a blank field.

That's not a coincidence. Attackers spray blank passwords because **blank passwords work** — especially in OT environments where PLCs, RTUs, HMIs and tank monitoring systems frequently ship with no default password and never get one set. These devices sit on operational networks for years, sometimes decades, with factory defaults intact.

The same credential list hitting my SSH honeypot is hitting exposed OT assets in energy infrastructure right now.

---

## The Global Picture

![Dynamic Attack Map](screenshots/<img width="576" height="284" alt="07_dynamic_attack_map png " src="https://github.com/user-attachments/assets/5af5b2cc-5290-4cd3-a68e-a51fe4a60b5d" />
)

Every bubble on this map is a source of attacks against simulated petroleum infrastructure over forty days. Bubble size represents attack volume.

North America. Europe. Asia. South America. Africa. Oceania.

No continent absent. No region safe. The attack surface isn't regional — it's planetary.

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

## What O&G Operators Should Do About This

### Right Now

**Change every default credential on every OT device.** PLCs. RTUs. HMIs. Tank gauges. Flow computers. Every device that shipped with `admin/admin` or a blank password — change it today.

**Disable SNMP v1/v2c.** Migrate to SNMPv3 with authentication and encryption. If SNMP isn't needed — disable it entirely.

**Patch your Cisco ASA VPN.** 174,000 attempts aren't going away. Known CVEs on unpatched ASA appliances are actively being exploited. Check your firmware version today.

**Add MFA to all VPN access.** A single compromised credential should not be sufficient for network access. Colonial Pipeline proved what happens when it is.

### In the Next 90 Days

Deploy OT-specific monitoring — solutions like Claroty, Dragos or Nozomi Networks provide behavioral detection that signature-based tools miss entirely.

Conduct an OT asset inventory. You cannot protect what you don't know exists. Identify every internet-adjacent OT device in your environment.

Review your firewall rules. Are any OT protocols directly internet-exposed? Modbus, S7comm, ENIP and BACnet have no business being reachable from the public internet.

---

## The Takeaway

I ran this honeypot for forty days. I got one million attacks.

The most targeted single piece of infrastructure was a simulated Cisco ASA VPN. The most targeted OT protocol was SNMP — reconnaissance, not exploitation yet. The most common password was nothing.

None of this is theoretical. These are real attacks from real IPs against real simulated petroleum infrastructure running on Azure.

The question for O&G operators isn't whether your infrastructure is being targeted. This data answers that question.

**The question is whether you'd know if someone got in.**

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

OT/ICS Cybersecurity Analyst with a background spanning pharmaceutical manufacturing and military infrastructure environments. Contributing Security Analyst with hands-on Splunk Enterprise Security expertise. CBRN Specialist — US Army National Guard. MS Cybersecurity and Information Assurance (In Progress). CEH | Secret Clearance.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/mheymann1)
[![GitHub](https://img.shields.io/badge/GitHub-mheymann1-black)](https://github.com/mheymann1)

---

*All data collected from attacks against infrastructure owned and operated by the author. No real OT systems were involved. Published for educational and research purposes.*
