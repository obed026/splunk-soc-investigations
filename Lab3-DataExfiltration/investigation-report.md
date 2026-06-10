**Incident Report — Data Exfiltration via Joomla CMS Analyst: Obed tekeste | Date: June 10, 2026 | Severity: Critical**

**What Happened**

While investigating outbound HTTP traffic, I found a single external IP — 40.80.148.42 — had pushed nearly 19MB of data to an internal machine at 192.168.250.70. That volume stood out immediately. Normal web traffic doesn't look like that.
What made it more interesting was that I recognized the IP. It was the same one from Lab 1 — the scanner that hit the Joomla site 13,415 times. This wasn't a coincidence. This was the same attacker, and the exfiltration happened before the brute force, not after.

**What the Date showed**

Digging into the traffic, the first thing that showed up was Acunetix — a web vulnerability scanner. The attacker ran an automated scan starting at 17:36, probing for weak points across the server. Random paths like /OD6xDhbF and /cgi-bin/le_check_v3.exe were fuzzing attempts looking for exploitable files.
Once they found their way in, the bulk of the data moved through two Joomla components — the search component at /joomla/index.php/component/search/ accounted for 13.42MB alone, and the mailto component moved another 1.14MB. The attacker was abusing built-in CMS functionality to pull data out rather than using obvious exploit tools that might get flagged.

**Attack Timeline**

The exfiltration ran from 5:35 PM to 6:20 PM on August 10, 2016 — nearly 45 minutes of sustained data transfer. The timechart showed consistent volume every 5 minutes throughout that window. That regularity points to automated tooling, not someone manually clicking around.

**The Full Attack Chain**

Connecting this back to Labs 1 and 2, the complete picture looks like this — the attacker scanned and exfiltrated data first, then brute forced the admin panel at 6:45 PM to establish deeper access, and the C2 beaconing on internal machines followed shortly after. One attacker, three techniques, one coordinated campaign.
MITRE ATT&CK Classification
•	T1190 — Exploit Public-Facing Application
•	T1041 — Exfiltration Over C2 Channel

**Detection Rule Built**

Any source/destination pair transferring more than 50MB of outbound data triggers the alert. Saved as "Large Outbound Data Transfer Detected" — scheduled every 15 minutes, High severity.

**What Should Have Prevented This**

A WAF with vulnerability scanner signatures would have blocked the Acunetix scan before it even started. Beyond that — rate limiting on Joomla search requests, outbound transfer monitoring, and network segmentation to prevent direct external access to internal machines. Any one of those controls breaks this attack chain.

