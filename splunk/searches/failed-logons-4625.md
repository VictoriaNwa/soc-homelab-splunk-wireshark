This SPL query detects failed logon attempts on virtual machine WS01.  
Windows uses EventCode **4625** for failed logons. My environment logs under main rather than directly under wineventlog.

index=main EventCode=4625

This helps identify repeated log on attempts, useful for recognizing potential brute force attacks early on.
