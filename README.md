# Windows-Process-Service-Enumeration
🛡️ Windows Process &amp; Service Enumeration Investigation


🎯 Objective

To perform structured enumeration of running processes and services on a Windows system, identify critical system components, and analyze potential security risks caused by misconfigurations or malicious abuse.


🖥️ Lab Environment

	•	Operating System: Windows 10
	•	Tools Used: Windows CMD, Task Manager, Services Console
	•	Privilege Level: Standard User + Administrator (for comparison)



🛠️ Tools & Commands Used

	•	tasklist
	•	tasklist /svc
	•	net start
	•	services.msc
	•	Task Manager
	•	Event Viewer


🔎 Step-by-Step Methodology


Step 1: Enumerate Running Processes

Command:

tasklist
<img width="1366" height="768" alt="PE" src="https://github.com/user-attachments/assets/23ec648c-d548-4aeb-ae6d-1f35b3cdcbc9" />

Purpose

To list all active processes currently running on the system.

Observation

	•	Identified system processes such as:
	•	svchost.exe
	•	lsass.exe
	•	explorer.exe
	•	Observed memory usage and session IDs.


Step 2: Map Processes to Services

Command:

tasklist /svc
<img width="1366" height="768" alt="SVC" src="https://github.com/user-attachments/assets/e1810c59-c8e3-49c5-86ca-f38fb4195570" />

Purpose

To determine which Windows services are running under each process.

Key Finding

Multiple services were running under svchost.exe.

This demonstrates how Windows groups services under shared host processes.


Step 3: Enumerate Active Services

Command:

net start

Purpose

To list currently running Windows services.

Observation

Identified services such as:

	•	Windows Defender Service
	•	DHCP Client
	•	DNS Client
	•	Remote Procedure Call (RPC)


Step 4: Analyze Services via GUI

Tool:

services.msc
<img width="1366" height="768" alt="SE" src="https://github.com/user-attachments/assets/468af992-ed9b-483b-a3f0-4c75cd49123e" />

Purpose

To review:

	•	Startup type (Automatic, Manual, Disabled)
	•	Service dependencies
	•	Logon account
	•	Recovery options

⸻

📊 Process & Service Deep Dive

Below is one process and one service analyzed in detail:

🔹 Process Analysis: lsass.exe
<img width="1366" height="768" alt="PE1" src="https://github.com/user-attachments/assets/b37afa76-5156-4887-a05a-a7b7556b33d4" />

Purpose

lsass.exe (Local Security Authority Subsystem Service) is responsible for:

	•	Enforcing security policy
	•	Handling user authentication
	•	Managing password validation

Ownership

Owned by the Windows operating system.

Security Concern if Misconfigured

If compromised:

	•	Attackers can dump credentials from memory.
	•	Enables privilege escalation.
	•	Common target in malware attacks.

Credential dumping tools frequently target this process.


🔹 Service Analysis: DNS Client

Purpose

Caches DNS names locally to improve performance and reduce network traffic.

Ownership

Managed by Windows Networking Services.

Security Risk if Misconfigured

	•	If disabled → DNS resolution delays
	•	If manipulated → Potential redirection to malicious domains
	•	Can assist attackers in persistence or data exfiltration


🚨 Security Risks Identified

1️⃣ Hidden malicious processes mimicking system names
2️⃣ Services set to “Automatic” without validation
3️⃣ Privilege abuse via service misconfiguration
4️⃣ Weak recovery options allowing repeated malicious execution



🛡️ Mitigation Strategies

	•	Monitor unusual process names (typosquatting like lsas.exe)
	•	Use endpoint detection tools
	•	Restrict service modification permissions
	•	Regularly audit startup services
	•	Monitor via SIEM platforms such as Splunk



🧠 What I Learned

	•	Process enumeration is foundational for threat hunting.
	•	Many services operate under shared host processes.
	•	Critical authentication processes must be monitored.
	•	Misconfigured services can be used for persistence.



💼 SOC Analyst Relevance

This investigation supports:

	•	Endpoint monitoring
	•	Malware detection
	•	Privilege escalation detection
	•	Service abuse detection
	•	Incident triage operations



🔬 Future Improvements

	•	Perform live monitoring with Sysmon
	•	Capture suspicious process activity using Wireshark
	•	Simulate malicious persistence in a lab environment
