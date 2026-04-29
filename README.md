Log Analysis – Compromised WordPress (Splunk)

Overview

This project documents the investigation of a compromised WordPress environment using log analysis in Splunk. The objective was to analyze web server access logs, identify suspicious activity, and determine how the attacker gained access to the system.

The challenge simulates a real-world incident response scenario, focusing on detecting exploitation attempts, malicious access patterns, and attacker behavior.

Objectives
Analyze web server access logs
Identify the attack vector used to compromise the system
Detect malicious requests and unusual patterns
Determine the web shell used by the attacker
Investigate attacker interaction with the system

Tools Used
Splunk (Log ingestion & analysis)
Linux (Kali environment)
Web server logs (access_log)

Investigation Process
1. Log Exploration
The dataset was ingested into Splunk and queried using:
index="access_log"
This provided a baseline view of all HTTP requests.

2. Identifying Suspicious POST Requests
Attackers often use POST requests to upload malicious files or exploit vulnerabilities.
index="access_log"
method=POST | stats count by uri
This helped highlight endpoints receiving unusual POST traffic.

3. User-Agent Analysis
To identify automated tools or suspicious clients:
index="access_log"
| stats count by useragent
This revealed:

Outdated browsers
Automated or scripted requests
Unusual traffic patterns

4. Attack Vector Identification
From the logs, the compromised entry point was traced to a vulnerable WordPress plugin:
 Simple File List 4.2.2
This plugin was exploited to gain unauthorized access.

5. Web Shell Detection
Further analysis revealed the uploaded malicious file:
 fr34k.php
This file acted as a web shell, allowing the attacker to execute commands on the server.

6. Final Activity Analysis
Tracking access to the web shell showed that the final interaction resulted in:
HTTP Status Code: 404
This suggests:

The file may have been removed
The attacker lost access
Defensive action may have been taken


Key Findings
The system was compromised via a vulnerable WordPress plugin
The attacker uploaded a PHP web shell (fr34k.php)
POST requests were used as the primary attack method
Logs revealed clear attacker interaction patterns
Final access attempt returned a 404 response

Screenshots

<img width="1366" height="560" alt="webscan2" src="https://github.com/user-attachments/assets/d3ea16f4-f24f-4788-a886-7d70fee1f88e" />
<img width="1366" height="550" alt="http-response" src="https://github.com/user-attachments/assets/7a61b26f-cfd8-4b2f-b2ad-0e55d6a75fb6" />
<img width="1366" height="632" alt="completed" src="https://github.com/user-attachments/assets/5bc79f42-bc7d-4927-8600-2da2d8094198" />

Lessons Learned
Always keep WordPress plugins updated
Monitor POST requests for unusual activity
Log analysis is critical for detecting breaches
Web shells are a common post-exploitation technique
User-agent analysis can reveal attacker tools
