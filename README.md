# Qakbot to Cobalt Strike: Threat Hunting Investigation

Investigation of a real-world Qakbot infection that escalated to Cobalt Strike command-and-control, reconstructing the attack from phishing email to domain controller exposure.

**Context:** CYB102 capstone (CodePath), Group 18 | Jul to Aug 2026
**Dataset:** [Malware-Traffic-Analysis, 2020-12-15](https://www.malware-traffic-analysis.net/2020/12/15/index.html) (not redistributed here)

## Key Findings

- **Attack chain:** phishing email → malicious ZIP → Qakbot execution → Qakbot C2 → Cobalt Strike → internal contact with the domain controller
- **Dwell time:** about 7 hours 15 minutes between Qakbot C2 establishment and Cobalt Strike escalation
- **Beaconing:** 6,241 connections from the infected host to a single C2 IP, at roughly 5-minute intervals
- **Detection gap:** VirusTotal showed low detection, while Zeek traffic analysis confirmed active C2

## Timeline

| Time | Event |
|---|---|
| 21:33:47 | Initial payload downloaded (disguised .jpg) |
| 21:41:28 | Qakbot C2 beacon established |
| 04:49:01 | Cobalt Strike escalation begins |

## Indicators of Compromise

| Stage | Indicator |
|---|---|
| Initial payload | `162[.]241[.]219[.]74` |
| Qakbot C2 | `78[.]101[.]199[.]138:995` |
| Cobalt Strike C2 | `172[.]241[.]27[.]244` |

## Methods and Tools

- **Splunk:** ingested HTTP, DNS, TLS, SMTP, and TCP traffic, and built a custom sourcetype to parse the original 2020 timestamps correctly
- **Zeek and Wireshark:** identified the victim host and analyzed beaconing behavior across ports 80, 443, and 8888
- **ANY.RUN:** sandboxed the suspicious file and observed process spawning and C2 communication
- **VirusTotal:** threat intelligence validation

## Incident Response Recommendations

1. Isolate the infected host
2. Block all C2 IPs
3. Disable compromised accounts
4. Rebuild the host and reset credentials
5. Update employee training and email filters

## My Contributions

- Analyzed the packet captures in Wireshark to identify indicators of compromise, including 3 malicious IPs tied to different stages of the attack (initial payload delivery, Qakbot C2, and Cobalt Strike C2)
- Collaborated with a team of six to draft the attack path, mapping the chain from the phishing email through Qakbot execution and Cobalt Strike escalation to domain controller exposure
- Cross-checked findings with teammates' Splunk, Zeek, and ANY.RUN results so the final timeline and IOC list were consistent

## Team

Group 18: Darren N, Christopher D, Tori C, Rohith A, Elshaday T

## Contents

- [Capstone presentation (PDF)](presentation/Qakbot-Capstone-Presentation.pdf)
