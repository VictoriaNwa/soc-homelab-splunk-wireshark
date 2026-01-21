# Issue — Forwarder Inactive

This issue occurred after completing the Splunk Universal Forwarder setup on the Domain Controller (DC). When verifying log ingestion in Splunk, DC logs were not appearing.

---

## Problem
After running the search in Splunk Web on Client1:
- index=main sourcetype=WinEventLog:Security | stats count by host

Only Client1 appeared as the host.  
The Domain Controller (DC) was not showing up, even after the forwarder installation and inputs were configured.
But: 
- DC should've been appearing as a host in Splunk
- Windows Security Event Logs from the DC should've be ingested and searchable

---

## Initial Checks
I confirmed: 
- Splunk Enterprise was accessible on Client1 (http://localhost:8000)
- Receiving was enabled on port 9997
- inputs.conf existed on the DC and contained WinEventLog inputs
- and restarted the SplunkForwarder service on the DC (Ran: splunk restart)

DC logs were still not visible in Splunk.

---

## Diagnosis
I checked the forwarder status on the DC command line:
- splunk list forward-server
The output showed the forwarder was configured but inactive, so the DC could not establish a connection to Client1.

To narrow down the issue, I ran the following on the DC in PowerShell as admin:
- Test-NetConnection 192.168.0.101 -Port 9997
- TcpTestSucceeded: returned False

## Fix
- Opened Windows Defender Firewall with Advanced Security on Client1
- Created a new Inbound Rule:
  - Rule Type: Port
  - Protocol: TCP
  - Port: 9997
  - Action: Allow the connection
  - Profiles: Domain, Private, Public
- Restarted the SplunkForwarder service on the DC

---

## Verification
After applying the firewall rule, I rechecked connectivity from the DC with the same, Test-NetConnection 192.168.0.101 -Port 9997, and it returned True. 
