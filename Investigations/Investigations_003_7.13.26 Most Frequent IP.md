# SIEM Finding: 
Most Frequent IP address 

## Summary:
167.99.154.193 was the most frequent source IP record in the last 100 security logs. 
Logs were obtained via Nginx access.log and ingested into Splunk for SIEM operations.
Out of 100 events, this IP accounted for 11 events. The events occured within seconds, 
therefore the events were likely automated scanner traffic. To further investigate,
CISCO Talos was used to search the IP address. The network owner was Digital Ocean, LLC
which is a cloud hosting platform with a neutral reputation, ASN AS14061.The findings further
confirm the conclusion that the activity is automated scanner traffic. 

The IP address successfully accessed robots.txt, the access.log responded with code 200. For this
project, the severity is low; However; in real-world practice a 200 response to a sensitive file is severe so
this SIEM finding will be classified as such.

## count / 100
11 (11%)

## Resource Accessed:
robots.txt

## Risk:
Severe - robots.txt accessed after enumeration

## Real World Response
Block the IP address via firewall configuration, review the contents of robots.txt to determine if sensitive information
is present, and implement rate limiting to limit attack frequency.
 

