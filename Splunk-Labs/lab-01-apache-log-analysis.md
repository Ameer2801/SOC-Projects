# Lab 01 – Apache Log Analysis (Splunk SOC Triage)
## Environment Set Up ##
- Installed Splunk Enterprise (Local Install)
- Uploaded Sample Apache access log file
- Index: Main
- Source type: apache_access
- Data_set entries: 10,000

## First Verification Query 
```spl
index=main sourcetype=apache_access
```
Note: Confirmed logs successfully ingested

## Total Events Query 
```spl
index=main sourcetype=apache_access | stats count
```
We got 10,000 events.

## Finding the Noisiest Ips 
```spl
index=main sourcetype="apache_access" | stats count as requests by clientip | sort - requests
```
- 66.249.73.135   482 requests
- 46.105.14.53    364 requests
- 130.237.218.86  357 requests

## Looking for Ips that have scanning behavior means too many 404 
```spl
index=main sourcetype="apache_access" status=404 | stats count as not_found by clientip | sort - not_found
```
- 208.91.156.11  60
- 144.76.95.39   14
- 66.249.73.135   8

## Suspicious Ip 

208.91.156.11 is the most suspicious ip cause it has too many 404 requests.
It might be automated probe scanning or directory scanning.

## Percentage calculation from normal request to 404 requests of this ip 
```spl
index=main sourcetype=apache_access clientip="208.91.156.11"
| stats count as total, sum(eval(status=404)) as not_found
| eval pct_404=round((not_found/total)*100,2)
```
Findings:
       208.91.156.11 have 60 total request and all of them are 404 not found ones so this strongly indicates suspicious behavior.

## URI Path that was being requested 
```spl
index=main sourcetype=apache_access clientip="208.91.156.11"
| stats count by uri_path
| sort - count
| head 15
```

Output:
/files/logstash/logstash-1.3.2-monolithic.jar

## Final Conclusion 

This request suggests the IP was:

Looking for a specific file

1. Possibly probing for exposed Logstash components

2. Attempting to access outdated or misconfigured infrastructure

Since:

Every request returned 404

It targeted a specific software-related file

2. It made 60 failed attempts

3. This is consistent with automated reconnaissance or scripted scanning.

This is not normal user browsing.

## SOC Response

- Validate IP reputation
- Monitor for repeated attempts
- Escalate if behaviour continues
- Consider firewall/WAF blocking if confirmed malicious
 

