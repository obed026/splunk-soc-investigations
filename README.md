# Splunk SOC Investigation Portfolio

Hands-on threat detection and incident response labs built using 
Splunk Enterprise and the BOTS (Boss of the SOC) v1 dataset — a 
real-world APT attack scenario published by Splunk. Each lab 
follows the full SOC workflow: detect, investigate, document, 
and build a detection rule.

## Environment
- **SIEM:** Splunk Enterprise 10.4 (local)
- **Dataset:** BOTS v1 attack-only dataset
- **Attack scenario:** August 2016 coordinated APT campaign
- **Log sources:** HTTP stream, DNS stream, firewall logs

## Labs

### Lab 1 — Brute Force Attack Detection
Investigated an automated credential stuffing attack against a 
Joomla CMS admin panel. Identified the attacker IP from HTTP 
traffic volume anomalies, confirmed successful compromise via 
HTTP 200 response analysis, and reconstructed a minute-by-minute 
attack timeline showing 411 failed login attempts in 90 seconds 
before the password cracked.

Built a scheduled Splunk alert that would have fired within the 
first 60 seconds of the attack.

**MITRE ATT&CK:** T1110 — Brute Force  
**Key skills:** HTTP log analysis, SPL, alert creation, timeline reconstruction  
**Detection rule:** Any IP with 10+ failed POSTs to admin login within 1 minute

---

### Lab 2 — C2 DNS Beaconing Detection
Identified three infected internal machines communicating with a 
C2 server via DNS tunneling. Flagged anomalous DNS behavior — 
thousands of requests to near-zero unique domains — and identified 
encoded gibberish query strings used to transmit beacon signals 
while hiding inside normal DNS traffic. Visualized the beacon 
interval pattern using Splunk timechart.

Built a scheduled alert to detect any internal host making 200+ 
DNS requests with fewer than 5 unique domains.

**MITRE ATT&CK:** T1071.004 — C2 over DNS  
**Key skills:** DNS log analysis, multivalue field handling, 
beaconing pattern visualization  
**Detection rule:** High-volume low-diversity DNS requests per host

---

### Lab 3 — Data Exfiltration Investigation
Traced nearly 19MB of data leaving the network through abused 
Joomla CMS components. Identified the attacker using Acunetix 
vulnerab
