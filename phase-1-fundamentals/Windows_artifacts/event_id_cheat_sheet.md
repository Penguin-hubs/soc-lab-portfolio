Windows Event ID Cheat Sheet for Threat Hunting :

This cheat sheet documents critical Event IDs (EIDs) used to detect post-exploitation activity, 
focusing on attacker actions after initial access has been achieved. These EIDs allow analysts to track execution,
persistence, and defense evasion techniques.

1. Process Monitoring (Execution & Activity)
   

These events track the execution of applications and scripts. Anomalies here often signal malware running or system utilities being abused.

EID 4688: A New Process Has Been Created (Security Log)

Detection Focus: This is the most important process event. Look for:

Unexpected Parent/Child Chains: Look for suspicious parent processes (e.g., svchost.exe launching powershell.exe).

Obfuscated Commands: Check the New Process Name and Command Line fields for unusual file paths or encoded strings.


EID 7045: A Service Was Installed in the System (System Log)

Detection Focus: Attackers often create a new service to maintain persistence. Look for services created by non-system 
accounts or with suspicious executable paths (e.g., from the temp or AppData folders).


EID 5156: The Windows Filtering Platform has permitted a connection (Windows Firewall)

Detection Focus: Indicates an application successfully created an outbound connection. Look for unexpected applications attempting to connect to external, non-corporate IP addresses—a sign of data exfiltration or Command and Control (C2).


2. Persistence and Defense Evasion
   

These events track the attacker's attempt to survive a reboot or remove their tracks.

EID 4657: A Registry Value Was Modified (Security Log)

Detection Focus: Critical for detecting persistence attempts. Look for modifications in known startup locations, specifically:

   HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

   HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run

Any new value pointing to a suspicious file is high priority.


EID 4672: Assign Special Privileges (Security Log)

Detection Focus: Indicates an account was granted administrative or system-level privileges. 
This is key for spotting Privilege Escalation attempts where a standard user gains control over the entire system.


3. The Attacker's Cover-Up

This is the ultimate sign of a skilled attacker attempting to hide their activity.

EID 1102: The Audit Log Was Cleared (Security Log)

Detection Focus: CRITICAL ALERT. A non-administrator account or an unexpected script clearing the Security Log is almost always malicious.
This is the attacker attempting to destroy the evidence of the breach. This $\text{EID}$ must trigger an immediate, high-priority investigation.
