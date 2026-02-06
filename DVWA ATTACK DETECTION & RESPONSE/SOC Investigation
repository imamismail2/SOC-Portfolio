
Attack Timeline:

Path Windows:
Steps and conmands:

1 - Identify the attack timeline in Apache logs
         notepad C:\xampp\apache\logs\access.log
    Filter for attacker IP
       findstr "10.10.10." C:\xampp\apache\logs\access.log


Find IOCs
Repeated Requests to:
      /dvwa/vulnerabilities/sqli/

      /login.php

     /security.php

High request frequency

Encoded payloads (%27, %23, %3D)

SOC finding: Clear malicious pattern consistent with SQL injection automation.


2 - Filter for SQL keywords:

serach: findstr /i "union select or 1=1" C:\xampp\apache\logs\access.log



Typical log entry:

GET /dvwa/vulnerabilities/sqli/?id=1%27+UNION+SELECT+user,password+FROM+users%23 HTTP/1.1

SOC Conclusion: Confirmed SQL injection exploitation

3- Identif Automation (sql fingerprint)

Look for sqlmap User-Agent
   sqlmap/1.9.8

SOC Severity: HIGH
Reason: Automated exploitation tool detected 

4 - Investigate Authentication abuse

Login brute / abuse check
search: findstr "login.php" C:\xampp\apache\logs\access.log

Indicators:
Multiple POSTs in short time
CSRF token reuse
Session cookie reuse

SOC assessment: Credentialed attacker abusing weak auth controls.

5 - Application Error log

   Apache error log: 

      C:\xampp\apache\logs\error.log

       serach for: findstr /i "mysqli warning fatal" C:\xampp\apache\logs\error.log
   Typical findings:

SQL syntax errors

PHP warnings

Stack traces (information disclosure)

SOC impact: Sensitive backend info leaked to attacker.

6 - Command Injection detection: 
     search acces logs for: findstr /i "cmd= whoami ipconfig net user" C:\xampp\apache\logs\access.log

    Look for: GET /dvwa/vulnerabilities/exec/?ip=127.0.0.1%26whoami

  SOC severity: CRITICAL
  Reason: Remote command execution attempt.   
  7 - Incident response actions 
Immediate containment

  Block attacker IP at firewall / IIS / Apache
  Disable DVWA application
  Reset exposed credentials
  Rotate session keys

Eradication
   Patch vulnerable code (prepared statements)
   Disable dangerous PHP functions:
          system
          exec
   Harden Apache config
   Remove DVWA from environment

Recovery
   Restore clean state
   Validate logs
   Continous monitoring for recurrence

8 - Detection Engineering 

  SQL Injection rule: 

 IF url_query CONTAINS ("union select" OR "' or '1'='1")
AND http_status = 200
THEN alert "Possible SQL Injection"

  Automation detection: 

IF user_agent CONTAINS "sqlmap"
THEN alert "Automated Web Exploitation Tool"



     
