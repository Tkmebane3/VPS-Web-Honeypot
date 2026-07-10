# INVESTIGATION-001: Environment File Enumeration
## Executive Summary
On July 9, 2026, an external host conducted automated reconnaissance
against the Internet-facing Nginx server. The source requested the homepage, 
which returned an expected 200 success code,
and subsequently tested four common environment file paths:
- `/.env`
- `/.env.local`
- `/.env.production`
- `/.env.bak`

All environment file requests returned HTTP 404 Not Found. No sensitive
files were exposed, and no evidence of successful exploitation was observed.

## Classification
- Incident type: Web reconnaissance and file enumeration
- Severity: Low
- Status: Closed — unsuccessful
- MITRE ATT&CK: T1595 — Active Scanning
- Data source: Nginx access logs

## Timeline
| Time (UTC) | Request | Status | Interpretation |
|---|---|---:|---|
| 00:18:33 | `GET /` | 200 | Initial server discovery |
| 00:18:35 | `GET /.env` | 404 | Environment-file probe |
| 00:18:42 | `GET /.env.local` | 404 | Local environment-file probe |
| 00:18:45 | `GET /.env.production` | 404 | Production configuration probe |
| 00:19:53 | `GET /.env.bak` | 404 | Backup configuration probe |

## Indicators
- Source IP: `144.172.103.227`
- Targeted paths: `/.env`, `/.env.local`, `/.env.production`, `/.env.bak`
- Protocol: HTTP
- Request method: GET

## Analysis
The sequence is inconsistent with normal browsing behavior. After confirming
that the web server responded successfully, the source tested multiple common
environment-file naming conventions. Such files may contain database
credentials, API keys, tokens, or other application secrets when improperly
deployed so the attacker was likely seeking access to this data.

The four targeted resources returned HTTP 404 responses, indicating that the
requested files were not publicly available. No successful authentication,
file disclosure, code execution, or persistence activity was observed.

## Disposition
No escalation was required because:

- All sensitive-resource probes failed.
- No HTTP 200 response was returned for the targeted files.
- No follow-on exploitation was observed.
- No evidence of data exposure or system compromise was identified.

## Lessons Learned
A request for a sensitive file is evidence of discovery attempts, not proof
of compromise. The HTTP response code and follow-on behavior are necessary
to determine whether the attempt succeeded.

## Security recommendations for environment file attacks
Environment files (such as .env, .env.local, and .env.production) should never reside within the web server's publicly accessible document root. Sensitive configuration files should be stored outside the web root so they cannot be retrieved over HTTP, even if their filenames are guessed. 
- Configure web servers to deny access to specific files
- Deploy WAF firewall configured to detect and block requests for sensitive files
- Implement .gitignore rules
- Securely store secrets in a secret management solution such as AWS Secrets Manager
- Alert when sensitive paths return HTTP 200 responses.
- Rate limiting for repetitive scans