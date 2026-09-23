# Investigation Title:
Environment File Enumeration Campaign

## Date: July 13, 2026

## Summary

A remote host performed automated reconnaissance against the web server by issuing more than 40 HTTP GET requests for environment files, configuration files, and debugging endpoints commonly associated with exposed application secrets.

All observed requests returned `404 Not Found`. No sensitive files were disclosed.

## Security Recommendations

- Generate a high-severity alert when requests for `.env` or similar sensitive files return HTTP `200`.
- Generate a medium-severity alert when one source requests multiple sensitive paths within a short time period.
- Correlate repeated reconnaissance activity from the same source IP across multiple web assets.
- Continue storing secrets outside the web root and enforce least-privilege access to configuration files.

## Classification

- **Incident Type:** Automated enumeration campaign
- **Severity:** Low
- **Status:** Closed — informational / unsuccessful
- **MITRE ATT&CK:** T1595 — Active Scanning
- **Data Source:** Nginx access logs

## Timeline

**2026-07-13 21:13 UTC**

More than 40 HTTP GET requests were observed targeting paths including `.env`, `.env.local`, `.env.production`, `sendgrid.env`, `twilio.env`, `backend/.env`, `phpinfo.php`, and `config.env`.

All observed requests returned HTTP `404`.

## Indicators

- **Source IP:** `213.209.159.154`
- **ASN:** AS208137
- **Network Owner:** Feo Prest SRL
- **HTTP Method:** GET
- **User-Agent:** `Opera/9.80`
- **Response:** `404 Not Found`

## Analysis

The activity is consistent with automated web enumeration attempting to identify exposed environment files, cloud credentials, backup files, and debugging endpoints.

The source issued a high volume of requests for common sensitive-resource names within a short period. None of the requested resources were retrieved because all observed requests returned `404`.

No evidence of credential exposure, file disclosure, or follow-on exploitation was identified in the reviewed logs.

## Disposition

**Closed — automated reconnaissance observed; no compromise identified.**

No immediate containment was required because the requests were unsuccessful and no further malicious activity was documented.

## Lessons Learned

Public-facing web servers may receive automated scanning. Secret-management practices and keeping sensitive files outside the web root reduce the risk of credential exposure. Alerting should distinguish routine low-severity scanning from events that indicate successful access or follow-on exploitation.

