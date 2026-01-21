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
- Opened DC's File Explorer
  - C:\Program Files\SplunkUniversalForwarder\etc\system\local and created inputs.conf as a text document
- Opened Notebook as administrator and typed this

[WinEventLog://Application]
disabled = 0

[WinEventLog://Security]
disabled = 0

[WinEventLog://System]
disabled = 0

  - Saved the file as **inputs.conf** NOT inputs.conf.txt
- Restarted the forwarder in command prompt as an admin
  - Ran: cd "C:\Program Files\SplunkUniversalForwarder\bin"
  - Ran: splunk restart

## Step 6 — Verify Log Ingestion on Client1
Confirmed logs were received using the following search in Splunk Web on Client1 (localhost:8000): 

SEE soc-homelab-splunk-wireshark/splunk/troubleshooting/forwarder-inactive-issue.md for forwarder issue faced.
