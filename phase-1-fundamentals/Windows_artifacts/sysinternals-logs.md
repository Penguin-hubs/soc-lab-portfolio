Sysinternals Process Analysis: Parent/Child Relationships

1. Objective
This task demonstrates the ability to use Process Explorer (part of the Sysinternals Suite) to analyze running processes and identify their execution lineage (parent/child relationships). This is a foundational skill for detecting "Living off the Land" attacks where malicious code runs under a seemingly legitimate process.

2. Procedure and Tool Used
   
Tool: Process Explorer (procexp64.exe) on a Windows 11 VM.

Concept: Every process is launched by a parent process. Malware often attempts to hide by spawning as a child of an unexpected or unusual parent.

Test Chain: To validate functionality, a deliberate three-level execution chain was created:

   1-The user launched the Command Prompt (cmd.exe) from the desktop.
   2-From within the cmd.exe window, the Notepad application (notepad.exe) was launched.

3. Analysis of Screenshot
The resulting process tree clearly shows the expected execution flow:

1-explorer.exe (Windows Shell) is the parent of the user-initiated cmd.exe.
2-cmd.exe is the direct parent of notepad.exe.

Conclusion: This validates the Process Explorer's capability to trace the flow of execution, confirming the tool is ready for use in threat hunting scenarios where we look for anomalies (e.g., word.exe spawning powershell.exe).

5. Evidence (Screenshot):

<img width="1131" height="820" alt="image" src="https://github.com/user-attachments/assets/a576f86e-82ad-497c-a52c-03738ac20532" />
