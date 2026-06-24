# Honeypot Investigation Log - Day 1 6/23/26
## Objective
The objective of this project is to deploy a basic web-facing honeypot environment on a DigitalOcean VPS to observe live reconnaissance activity, web scanning behavior, and unauthorized access attempts from attackers.

## Environment
* Ubuntu VPS hosted on DigitalOcean
* Nginx web server for traffic management effeciency, web content serving, and for monitoring access logs. 
* Public IP address assigned from Digital Ocean
* Static HTML trading platform landing page ("TradeMaster Pro") as the Honeypot

## Activities Performed

### 1. Service Enumeration
Investigated running services using systemctl and process enumeration. This VPS previously ran live automated trading scripts. 

What was Discovered:

* Nginx service running
* Legacy Creed trading bot service running
* Python process listening on port 5000

### 2. Port Enumeration
Used:
ss -tulpn (socket statistics) to find what services are actively open and listening

What was Identified:

* Port 22 (SSH)
* Port 80 (Nginx)
* Port 5000 (Legacy Creed application)

### 3. Root Cause Investigation
It was found that browser requests were still displaying content from the retired Creed application. This was a critical bug that prevented the Honeypot from being available. 

Troubleshooting method:

* Systemd service configuration
* Nginx configuration
* Listening ports

Determined that Nginx was previously configured as a reverse proxy forwarding traffic to port 5000, therefore traffic attempting to access the honeypot's port 80 was being redirected to the dead port 5000. 

### 4. Nginx Reconfiguration
Removed reverse proxy behavior and configured Nginx to serve static content from:
/var/www/html
Deployed a basic TradeMaster Pro login page.

### 5. Network Troubleshooting
External users could not access the site despite Nginx operating normally.

Investigation included:

* Nginx status validation
* Port verification
* Firewall verification
* Packet path analysis

Discovered a NAT redirect rule forwarding inbound TCP port 80 traffic to port 5000.
Removed redirect rule and restored direct Nginx access.

### 6. Fingerprinting Exercise
Performed manual web application fingerprinting.

Observed:
* Server: nginx/1.24.0 (Ubuntu)
* HTTP status codes
* Browser request headers
* User-agent information

Reviewed opportunities to reduce information disclosure through Nginx configuration.

### 7. Logging Validation
Confirmed traffic collection through:

/var/log/nginx/access.log

Observed successful requests from:

* Localhost
* VPS public IP
* External workstation

## Skills Practiced
* Linux administration
* Nginx configuration
* Network troubleshooting
* Service enumeration
* Port analysis
* HTTP response analysis
* Web server fingerprinting
* Log analysis
* Root cause investigation

## Next Steps
* Monitor access logs for reconnaissance activity
* Analyze scanning behavior
* Create custom logging dashboards
* Investigate common web enumeration attempts for example:

  * /wp-admin
  * /.env
  * /phpmyadmin
* Expand honeypot functionality for credential collection simulation and attack pattern analysis
