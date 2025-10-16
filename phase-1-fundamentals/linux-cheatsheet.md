Linux Triage Protocol: Security Analyst Reference

This document serves as a consolidated reference of fundamental Linux commands for system investigation, log analysis, and initial incident response activities, combining essential SOC triage techniques with practical skills gained from training exercises (OverTheWire, TryHackMe).

1. System Status and Network Triage (Current State Assessment)

These commands are the first priority for assessing the immediate health of a system and identifying active anomalies.

ps aux  
Purpose: Displays all running processes (all users, full format).
Application: Used to identify abnormal processes, unauthorized executables, or suspicious user-owned tasks.

top
Purpose: Provides a real-time monitor of system resources (CPU and Memory).
Application: Critical for observing resource spikes indicative of hidden malicious activity (e.g., crypto miners or high-volume data exfiltration).

netstat -tuln or ss -tuln
Purpose: Lists all listening network sockets.
Application: Crucial for identifying unauthorized open ports (e.g., backdoors or CnC communication waiting for a connection).

ip addr
Purpose: Verifies the host's IP address and network interface configuration.
Application: Used for establishing the system's context and reachability on the network.

last
Purpose: Examines the history of user logins/logouts.
Application: Used to find unauthorized sessions or access attempts outside normal working hours.

df -h
Purpose: Displays available disk space (human-readable format).
Application: Helps detect potential storage saturation from data dumps or large file exfiltration attempts.

2. Log Analysis and File Hunting: 

These commands are used for searching for artifacts, filtering large logs, and determining file integrity.
File Search (find)

find / -name filename: Searches for a specific file across the entire file system (OTW L5).
find . -user username: Finds files whose ownership is attributed to a specific user (Investigating compromised accounts).

File Filtering (grep)
grep "ERROR" file.log: Searches for a specific text pattern or string within any log file.

Uniqueness Filter (sort & uniq)
sort file.txt | uniq -u: Extracts lines that are present only once. Useful for filtering noise and identifying unique artifacts (OTW L8).

File Properties
file [filename]: Determines the actual file type ($\text{ASCII}$ text, $\text{ELF}$ executable, etc.). Mandatory before opening a suspicious file.

ls -l: Displays detailed file permissions (e.g., -rwx r-- r--). Checks for improper configurations or executable scripts.

Real-time Monitoring
tail -f file.log: Streams the content of a log file in real-time, allowing dynamic observation of events as they occur.

3. Advanced Decoding and Obfuscation: 

These utilities are essential for manipulating and decoding data that has been intentionally concealed by threat actors.

Base64 Decoding
base64 -d file.txt: Decodes a file encoded in Base64, a highly common technique for concealing scripts and payloads (OTW L10).

Extracting Strings
strings binary_file: Extracts all printable character strings from binary data, constituting a key step in preliminary forensic analysis (OTW L9).

Character Translation (ROT13)
echo "..." | tr 'A-Z' 'N-Z': Executes character substitution within a stream, enabling the reversal of simple ciphers (OTW L11).

Error Suppression
command 2> /dev/null: Redirects the standard error stream (2) to the null device, suppressing error messages to maintain clean command output (OTW L6).

