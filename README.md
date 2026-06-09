# Splunk SOC Investigation Portfolio

A collection of hands-on threat detection and incident investigation 
labs using Splunk Enterprise and the BOTS (Boss of the SOC) v1 dataset 
— a real-world attack scenario dataset published by Splunk.

## Environment
- Splunk Enterprise (local)
- Dataset: BOTS v1 (attack-only dataset)
- Attack timeframe: August 2016 APT scenario

## Labs

### Lab 1 — Brute Force Attack Detection
Investigated an automated brute force attack against a Joomla CMS 
admin panel. Identified the attacker IP, traced the full attack 
timeline, confirmed successful compromise via HTTP response analysis, 
and built a scheduled Splunk alert that would have fired within 60 
seconds of the attack starting.

**Key skills:** HTTP log analysis, SPL queries, alert creation, 
incident timeline reconstruction


## Skills Demonstrated
- Splunk SPL query writing
- Threat detection and alert building
- Incident investigation and timeline analysis
- Security log analysis (HTTP, DNS, firewall)
- Written incident reporting
