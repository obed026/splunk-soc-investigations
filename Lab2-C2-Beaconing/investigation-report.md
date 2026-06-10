****Incident Report — C2 DNS Beaconing Analyst: Obed Tekeste | Date: June 10, 2026 | ****

**Severity**: High

**What Happened**
Three internal machines were generating thousands of DNS requests to nearly zero unique domains — a pattern that doesn't match any normal user behavior. The top offender, 192.168.250.100, made 3,418 DNS queries. That's what flagged it first.

**What the Data Showed**

The queries weren't going to real domains. The top two were gibberish strings — FHFAEBEECACACACACACACACACACAAAA queried 456 times and EJFDEBFEEBFACACACACACACACACACAAA queried 120 times. The repeating ACACAC pattern is encoded data — this is DNS tunneling, where malware hides C2 communication inside DNS traffic to bypass firewalls.
The beaconing visualization showed a massive initial burst at 5:54 PM on August 10 followed by regular interval spikes — consistent with malware checking in on an automated timer.

**MITRE ATT&CK Classification**

 T1071.004 — Command and Control via DNS Application Layer Protocol

**Detection Rule Built**
 Any internal IP making more than 200 DNS requests with fewer than 5 unique domains triggers the alert. Saved as "Suspicious DNS Beaconing Detected" — scheduled, High severity.
What Should Have Prevented This
 DNS filtering to block known malicious domains, DNS inspection to flag non-standard query patterns, and network segmentation to limit which machines can make external DNS requests directly.
