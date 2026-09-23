# Investigation Title:
Environment File Enumeration

## Date: July 9, 2026

## Summary

An external host conducted automated reconnaissance against the Internet-facing Nginx server. After successfully requesting the public homepage, the source tested four common environment-file paths: `/.env`, `/.env.local`, `/.env.production`, and `/.env.bak`.

All environment-file requests returned `404 Not Found`. No sensitive files were exposed, and no evidence of successful exploitation was identified.

## Security Recommendations

- Keep environment and configuration files outside the publicly accessible web root.
- Configure the web server to deny access to sensitive configuration files.
- Use a WAF to detect and block requests for sensitive paths where appropriate.
- Maintain `.gitignore` rules to reduce accidental publication of secrets.
- Store application secrets in a dedicated secret-management solution.
- Alert when sensitive paths unexpectedly return HTTP `200`.
- Consider rate limiting for repetitive enumeration activity.

## Classification

- **Incident Type:** Web reconnaissance / environment-file enumeration
- **Severity:** Low
- **Status:** Closed — unsuccessful
- **MITRE ATT&CK:** T1595 — Active Scanning
- **Data Source:** Nginx access logs

## Timeline

| Time (UTC) | Request | Status | Interpretation |
|---|---|---:|---|
| 00:18:33 | `GET /` | 200 | Initial server discovery |
| 00:18:35 | `GET /.env` | 404 | Environment-file probe |
| 00:18:42 | `GET /.env.local` | 404 | Local environment-file probe |
| 00:18:45 | `GET /.env.production` | 404 | Production configuration probe |
| 00:19:53 | `GET /.env.bak` | 404 | Backup configuration probe |

## Indicators

- **Source IP:** `144.172.103.227`
- **Targeted Paths:** `/.env`, `/.env.local`, `/.env.production`, `/.env.bak`
- **Protocol:** HTTP
- **Method:** GET
- **Observed Responses:** `200`, `404`

## Analysis

The request sequence is inconsistent with normal browsing behavior. After confirming the web server responded successfully, the source tested multiple common environment-file naming conventions that may expose credentials, API keys, tokens, or other secrets when improperly deployed.

All targeted environment-file requests returned `404`, indicating the requested resources were not publicly available. No successful authentication, file disclosure, code execution, persistence, or follow-on exploitation was observed.

## Disposition

**Closed — reconnaissance observed; no compromise identified.**

No escalation was required because all sensitive-resource probes failed, no targeted file returned HTTP `200`, and no evidence of data exposure or system compromise was identified.

## Lessons Learned

Requests for sensitive files indicate discovery activity, not proof of compromise. Response codes and follow-on behavior are necessary to determine whether enumeration attempts were successful.