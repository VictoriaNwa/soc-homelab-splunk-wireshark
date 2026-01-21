This SPL query detects successful logon attempts on virtual machine DC.
Windows uses EventCode 4624 for successful logons.

index=main EventCode=4624

This helps identify regular log on activity. This would be useful for recognizing a logon found an account that hasn't been accessed in a while, which may raise flags.

Also ran: index=main EventCode=4624 | table_time Account_Name Logon_Type

Account_Name -> specified which account tried to login to WS01
Logon_Type -> specified how it was attempted
- 5 (Service) : a system or service autheniticated in the background
- 7 (Unlock) : unlocks an already logged in session that was locked

I also learned about SYSTEM, LOCAL SERVICE, and DWM-1 through Account_Name parameter.

SYSTEM -> highest privilege for OS processes
LOCAL SERVICE -> low privilege for background processes
DWM-1 -> account that manages Desktop Window Manager processes to handle things like visual effects, etc.
