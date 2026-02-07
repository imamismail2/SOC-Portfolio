MITRE ATT&CK Mapping – DVWA Web Application Incident
Overview

The following table maps observed attacker behaviors during the DVWA web application compromise to MITRE ATT&CK Enterprise techniques.
This mapping reflects real-world adversary tradecraft, despite occurring in a controlled lab environment.

Technique Mapping Table
Attack Phase	Observed Activity	MITRE Tactic	Technique ID	Technique Name
Initial Access	Exploitation of vulnerable web application (DVWA)	Initial Access	T1190	Exploit Public-Facing Application
Execution	SQL injection payloads executed via web input	Execution	T1059.007	Command and Scripting Interpreter: SQL
Credential Access	Extraction of credential hashes from database	Credential Access	T1552.001	Unsecured Credentials: Credentials in Files
Execution	OS commands executed via command injection	Execution	T1059	Command and Scripting Interpreter
Discovery	Database enumeration via information_schema	Discovery	T1083	File and Directory Discovery
Collection	Extraction of sensitive data (users table)	Collection	T1005	Data from Local System
Credential Access	Password hash harvesting for offline cracking	Credential Access	T1003	OS Credential Dumping
Resource Development	Use of automated exploitation tooling	Resource Development	T1588.002	Obtain Capabilities: Tool
Command & Control (Simulated)	Repeated automated HTTP requests	Command and Control	T1071.001	Application Layer Protocol: Web
Defense Evasion	URL-encoded payloads to bypass filters	Defense Evasion	T1027	Obfuscated/Compressed Files and Information

 Detailed Technique Breakdown
 ---T1190 – Exploit Public-Facing Application

Observed Evidence:

SQL injection against /dvwa/vulnerabilities/sqli/

Command injection against /dvwa/vulnerabilities/exec/

SOC Detection Sources:

Apache access logs

WAF (if deployed)

SIEM HTTP anomaly alerts

---T1059.007 – Command and Scripting Interpreter: SQL

Observed Evidence:

UNION SELECT user,password FROM users

Enumeration of information_schema

Impact:

Full database compromise

Credential disclosure

---T1552.001 – Unsecured Credentials

Observed Evidence:

Extraction of hashed credentials from users table

SOC Note:

Hashes represent credential material and should be treated as compromised

--- T1059 – Command and Scripting Interpreter

Observed Evidence:

OS-level commands executed:

whoami

ipconfig

net user

Severity:

Critical (Remote Command Execution)

--- T1588.002 – Obtain Capabilities: Tool

Observed Evidence:

Use of sqlmap for automated exploitation

Identified via HTTP User-Agent string

SOC Detection Tip:

Alert on known offensive tooling User-Agents

--- T1071.001 – Application Layer Protocol: Web

Observed Evidence:

Exploitation conducted entirely over HTTP

No malware dropped; “living off the app”



ATT&CK Tactic Coverage Summary
Tactic	Coverage
Initial Access	✅
Execution	✅
Credential Access	✅
Discovery	✅
Collection	✅
Defense Evasion	✅
Command & Control	⚠️ Simulated
Persistence	❌ Not observed
Privilege Escalation	❌ Not observed
Lateral Movement	❌ Not observed

Detection Engineering Opportunities
Example SIEM Detections
IF url_query CONTAINS ("union select" OR "information_schema")
THEN alert "SQL Injection Attempt"

IF user_agent CONTAINS "sqlmap"
THEN alert "Automated Web Exploitation Tool Detected"

IF request_uri CONTAINS "/vulnerabilities/exec"
AND query CONTAINS ("%26whoami" OR "%26ipconfig")
THEN alert "Command Injection Attempt"

🧠 How to Explain This in an Interview

“I mapped observed attacker behavior to MITRE ATT&CK, identifying Initial Access via T1190, Execution through SQL and OS command injection, and Credential Access through database extraction. This allowed me to prioritize detections and recommend mitigations aligned with ATT&CK tactics.”

That answer hits SOC, IR, and detection engineering expectations.
