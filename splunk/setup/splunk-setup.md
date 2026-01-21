# Splunk Setup Notes

These are the initial setup steps I followed to install Splunk Enterprise and prepare WS01 for log analysis.

---

## Step 1 — Verify WS01 Networking
- Enabled Adapter 2 (NAT) for internet access
- Left Adapter 1 as Host-Only for AD connectivity

---

## Step 2 — Install Splunk Enterprise
- Downloaded Splunk Enterprise Free from splunk.com
- Ran installer with default settings
- Logged in at http://localhost:8000

---

## Step 3 — Add Windows Event Logs
Clicked:
Add Data -> Monitor -> Local Event Logs (on splunk.com)

Enabled:
- Application
- Security
- System
- NEXT until SUBMIT

---

## Step 4 — Confirmed Splunk Search Functionality
Initial query used to test log ingestion:
index=main EventCode=4625

---

## Troubleshooting Reference

If you want to see issues I encountered during installation and configuration, along with how I fixed them:
See: splunk/troubleshooting/issues.md
