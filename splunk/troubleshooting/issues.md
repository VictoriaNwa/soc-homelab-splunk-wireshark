# Splunk Troubleshooting Notes

This file documents problems I ran into while setting up Splunk and how I fixed them.

---

## 1. No Internet on WS01
**Problem:** WS01 only had Host-Only adapter.  
**Fix:** Added NAT adapter (Adapter 2) from WS01 settings on Oracle Virtual Box Page to allow external traffic.

---

## 2. nslookup google.com failing
**Cause:** WS01 was using DC01's DNS for external lookups.  
**Fix:** NAT adapter automatically assigned public DNS (8.8.8.8) the next morning and resolved correctly on its own.

---

## 3. WS01 freezing during Splunk install
**Fix:** Increased RAM to 3072 MB and CPU to 2 cores.

Figured maybe the space splunk took was a little too much for the space I'd allocated.

---

## 4. Splunk “Local Event Logs” page showing 404 (not found)
**Fix:** On splunk website, go to Settings -> Add Data -> Monitor -> Local Event Logs.

I originally was accessing it from Settings -> Data Inputs -> Local event log collection (which generated the page error).

---

## 5. Windows Security Log was completely empty (0 bytes) causing query to return 0 results on Splunk
**Cause:** Security log was not writing any events.  
**Fix:** Enabled Windows auditing + created missing registry value:

Enabling Windows Auditing: 
 - Opened Windows Event Viewer on VM to check if it was logging events at all and it was not. 
 - Opened Local Security Policy.  
 - Path: Local Polices/Audit Policy
 - Activated:
   - Audit account logon events (Success/Failure)
   - Audit logon events (Success/Failure)
   - Also activated Process tracking (Success)
Didn't want to turn on too many because it may be too much for VM to handle.

Creating Mission Registry Value:
- Opened up Registry Editor
- Path: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Security
- Right-click -> New -> DWORD (32-bit)
  - Named it "Start" and set = 1
 
Reboot required.

---

## 6. localhost:8000 refused to connect after reboot
**Fix:** Checked Services on VM. 
- Stopped Splunkd Service
- Started it again.

Issue still occured. But eventually reloaded itself and connected. I think splunk just takes some time to fully start up, after VM boots up.

---

## 7. Query still returned no results
**Cause:** Windows logs were stored in "main" index, not "wineventlog".  
**Fix:** Identified correct index using:

- index=* | stats count by index (typed this Search and Reporting search bar on Splunk)
  - "main" is the only one that showed up.

So instead of using 
- index=wineventlog EventCode=4625

I used 
- index=main EventCode=4625 
  - and it began to show logs (correlated 21 results in the past day)
