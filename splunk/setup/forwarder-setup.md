# Splunk Forwarder Setup Notes

These are the steps I followed to install the Splunk Universal Forwarder on the Domain Controller (DC) and forward logs to Client1 for centralized analysis.

---

## Step 1 — Verify Client1 Is Ready to Receive Logs
- Opened Splunk Web on Client1 at http://localhost:8000
- Navigated to Settings → Forwarding and Receiving
- Enabled receiving on port **9997**

---

## Step 2 — Download Splunk Universal Forwarder
- Downloaded Splunk Universal Forwarder (Windows, 64-bit) on host laptop since DC has no NAT adapter
- Drag and dropped the installer to the DC (Downloads folder)

---

## Step 3 — Install Universal Forwarder on DC
- Ran the installer
- Accepted the license agreement
- Left installation options as default -> NEXT
- Left Deployment Server page blank
- Configured Receiving Indexer:
  - Hostname/IP: 192.168.0.101 (Client1 Internal Network IP)
  - Port: 9997
- Created forwarder admin credentials
- Completed installation

---

## Step 4 — Confirm Forwarder Service Status
- Opened services.msc on the DC
- Verified SplunkForwarder service was running

---

## Step 5 — Enable Windows Event Logs on DC
- Opened Notebook app as an administrator and opened file
  - C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf (I created inputs.conf)
  - Typed this (to enable fowarding for the logs from DC to Client1)
[WinEventLog://Application]
disabled = 0

[WinEventLog://Security]
disabled = 0

[WinEventLog://System]
disabled = 0
  - Saved the file
- Opened command prompt as an admin
  - Ran: cd "C:\Program Files\SplunkUniversalForwarder\bin"
  - Ran: splunk restart

## Step 6 — Verify Log Ingestion on Client1
Confirmed logs were received using the following searches in Splunk Web:
