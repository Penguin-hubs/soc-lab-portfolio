
1. Threat Profile

Attack: RDP Brute Force 

Tactic: Credential Access (T1110) 

Description: An attacker attempts to gain unauthorized access to a system by systematically guessing
login credentials over the Remote Desktop Protocol (port 3389/TCP) until a valid username/password combination is found. 
Successful compromises grant the attacker immediate, interactive access to the host.

2. Key Detection Signature (Event Log)
   
The main detection signature revolves around Event ID 4625 in the Windows Security Log. The alert is triggered when:


Log Type: Security Log

Event ID: 4625 (An account failed to log on)

Logon type: 10 (Remote Interactive), which confirms the RDP vector.

Alert Condition: Excessive number of failures from the same Source IP within a limited time period (e.g : 100+ attempts within 5 minutes).


3. Triage Procedure for Junior Analyst
   
The following steps are taken to validate and triage a potential RDP Brute Force alert:

Step 3.1: Log Filtering and Validation

Filter by Failure: Filter the Windows Security Log or SIEM data only for Event ID 4625 (Account Logon Failed).

Verify RDP Vector: Within the 4625 event details, ensure the Logon Type is 10 (Remote Interactive).

This validates the attempt was performed via RDP over the network.

Confirm Failure Reason: Check the Failure Sub Status Code (e.g., 0xc000006a) to ensure the failure reason is "bad password or username," which
represents a password-guessing attempt rather than a network failure.

Step 3.2: Identifying the Anomaly

Establish Threshold: Group the filtered 4625 events by the Source Network Address. Once one Source IP exceeds a predetermined 
threshold (e.g., 100 or more failed attempts in a 5-minute time frame), confirm the event as a Brute Force attack.

Check for Breach: Search the timeline around the brute force time window for Event ID 4624 (Successful Logon) from the same suspect Source IP.
A single 4624 event following a steady pattern of 4625s is an indicator of a successful breach.

Step 3.3: Immediate Response Action (Block and Alert)

Scenario 1: Attack Failed (Only 4625s present)

Critical Action: Block the Source Network Address at the firewall/network perimeter immediately.

Rationale: Stop the attack prior to success. Alert the L2 team for awareness.


Scenario 2: Attack Succeeded (Event ID 4624 is present)

Critical Action: Block the Source Network Address AND quarantine the affected host from the network ASAP.

Rationale: Contain the active threat to prevent lateral movement and further compromise, then initiate the Incident Response process.
