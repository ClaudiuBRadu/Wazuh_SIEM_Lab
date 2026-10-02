# Wazuh SIEM Home Lab

I built this lab environment to get hands-on experience with SIEM operations, specifically focusing on Wazuh. My goal was to go beyond a standard installation by actively generating attacks, writing custom detection rules, and tuning the system to catch specific behaviors.

Infrastructure Setup:
-Manager: Ubuntu Linux instance running on Oracle Cloud.
-Endpoint: Physical Windows machine with the Wazuh agent installed.
-Networking: I configured Oracle's Security Lists and local `iptables` rules to allow agent-manager traffic exclusively over TCP ports 1514 and 1515.

What I Tested:
Once the agent was enrolled and sending telemetry, I ran several scenarios to see how Wazuh reacts out of the box:
-FIM (File Integrity Monitoring): Tracked system file changes in real-time.
-Log Clearing: Ran `wevtutil cl Security` to trigger Event ID 1102 (a common evasion technique).
-Malware Detection: Dropped an EICAR test file to verify the integration between Windows Defender and Wazuh.
-Persistence & Brute-Force: Created and deleted hidden accounts, and simulated rapid failed logon attempts.

SIEM Tuning (Custom Rules):
By default, Wazuh treats repeated failed logons as a "Low" severity event (Rule ID 60122). To practice rule tuning, I wrote a custom rule in the backend that overrides this default. If the failed logon attempt targets a specific honeypot account I created (`HackerTest`), Wazuh now immediately escalates the alert to **Critical** (Level 15). You can see the rule logic in `local_rules.xml`.