
Windows Event ID Cheat Sheet for Threat Hunting

This cheat sheet documents notable Event IDs (EIDs) for threat hunting post-exploitation activity, focusing on attacker activity following confirmed initial access. These EIDs allow analysts to track execution, persistence, and defense evasion techniques.


1. Process Monitoring (Execution & Activity)

These events track application and script execution. Anomalies here are generally malware execution or system utilities being abused.

EID 4688: A New Process Has Been Created (Security Log)

Detection Focus: Most important process event. Look for:

Unexpected Parent/Child Chains: Look for unusual parent processes (e.g., svchost.exe spawning powershell.exe).

Obfuscated Commands: Look in the New Process Name and Command Line fields for suspicious file paths or encoded strings.

EID 7045: A Service Was Installed in the System (System Log)

Detection Focus: Attackers will often create a new service for persistence. Look for services installed by non-system accounts or with unusual executable paths (e.g., from the temp or AppData folders).

EID 5156: The Windows Filtering Platform has allowed a connection (Windows Firewall)

Detection Focus: Indication that an application has made a successful outbound connection. Look for suspicious applications attempting to connect to external, non-corporate IP addresses—a C2 or data exfiltration indicator.


2. Persistence and Defense Evasion

These events track the attacker attempting to persist through a reboot or wipe his tracks.

EID 4657: A Registry Value Was Modified (Security Log)

Detection Focus: Critical to identify persistence attempts. Look for additions in known startup locations, i.e.:

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run

Any new value that points to a suspicious file is high priority.

EID 4672: Assign Special Privileges (Security Log)

Detection Focus: Logs when an account was granted administrative or system-level privileges. Most important for detecting Privilege Escalation attempts when a normal user gains control of the entire system.


3. The Attacker's Cover-Up

This is the last sign of an advanced attacker attempting to hide his trail.

EID 1102: Audit Log Was Cleared (Security Log)

Detection Focus: CRITICAL ALERT. A Security Log clearing by a non-administrator account or an unexpected script is almost certainly malicious. This is the attacker attempting to erase the trail of the intrusion. This EID should trigger an immediate, high-priority investigation.
