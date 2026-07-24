# Investigation 002: Environment File Enumeration Campaign

## Summary
A remote host performed automated reconnaissance against the web server by requesting numerous environment and configuration files commonly associated with exposed application secrets. No sensitive files were disclosed. All requests resulted in HTTP 404 responses.

## Security Recommendation Summary
• Generate a high severity alert when requests for ".env" or similar sensitive files receive an HTTP 200 (Success) response.

• Generate a medium severity alert when a single source requests multiple sensitive paths (.env, phpinfo.php, config.env, .git) within a short time period.

• Correlate repeated reconnaissance activity from the same source IP across multiple web assets.

• Continue storing secrets outside the web root and enforce least-privilege access to configuration files.

## Classification
Incident Type: Enumeration campaign
Severity: Low
Status: Unsuccessful (404 response)
Data tool: Nginx logs
MITRE ATT&CK:
- T1595 – Active Scanning

## Timeline
2026-07-13 21:13 UTC

Observed over 40 HTTP GET requests targeting:
- .env
- .env.local
- .env.production
- sendgrid.env
- twilio.env
- backend/.env
- phpinfo.php
- config.env

## Indicators
Source IP:
213.209.159.154

ASN:
 AS208137 tracks to Feo Prest SRL

HTTP Method:
GET

User Agent:
Opera/9.80

Response:
404 Not Found

## Analysis
The activity is consistent with an automated web enumeration tool attempting to identify exposed environment files, cloud credentials, backup files, and debugging endpoints. ASN tracks to cloud hosting infrastructure. No files were retrieved. 

## Disposition
No compromise identified. Requests were unsuccessful and no further malicious activity followed
Status:
Closed – Informational

## Lessons Learned
Public-facing web servers are continuously scanned by automated tools regardless of organization size. Proper secret management and removal of sensitive files from the web root prevented credential exposure. Alerts should be properly filted to discern between low severity scanner traffic and more severe threats.

