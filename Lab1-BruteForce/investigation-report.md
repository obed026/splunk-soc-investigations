**Incident Report — Joomla Admin Brute Force Attack Analyst: Obed Tekeste | Date: June 9, 2026 | Severity: High**

**What Happened**

While reviewing HTTP traffic in the BOTS v1 dataset, I noticed one external IP — 23.22.63.114 — had over 1,200 hits concentrated almost entirely on /joomla/administrator/index.php. That immediately stood out. Normal users don't hammer an admin login page over a thousand times.
I dug deeper and filtered to just POST requests from that IP. What came back was a wall of 303 responses — failed login redirects — firing multiple times per second starting at 17:45:21. No human types that fast. This was automated.
The brute force ran for about 90 seconds across two full minutes. In that window the tool threw over 400 login attempts. Then at 17:46:51 the POSTs stopped completely. I checked why — and found 823 HTTP 200 responses to the same admin page following the stop. The attacker was in.

**Detection Rule I Built**

I wrote a SPL query to catch this pattern automatically — any source IP failing more than 10 logins in a single minute against the admin panel triggers the alert:
splunk
index=botsv1 sourcetype=stream:http
uri_path="/joomla/administrator/index.php"
http_method="POST" status=303
| bucket _time span=1m
| stats count by _time, src_ip
| where count > 10
Running this against the dataset flagged 23.22.63.114 in both the 17:45 and 17:46 minute windows — 199 and 212 failed attempts respectively. This rule would have fired within the first 60 seconds of the attack, before the password cracked.
I saved it as a scheduled Splunk alert called "Brute Force Login Detection - Joomla Admin" set to run every 5 minutes.

**Secondary Finding**

A second IP — 40.80.148.42 — was hitting the Joomla site 13,415 times across multiple endpoints during the same window. Based on the pattern across /joomla/index.php, the search component, and the mailto component, this looks like an automated web scanner doing reconnaissance while the brute force ran in parallel. Likely the same threat actor using two tools simultaneously.

**What Should Have Prevented This**

Account lockout after 5 failed attempts would have stopped this cold. Beyond that — MFA on the admin panel, IP allowlisting for /joomla/administrator/, and a WAF rate-limiting rule on login POST requests. Any one of those controls breaks the attack chain.

