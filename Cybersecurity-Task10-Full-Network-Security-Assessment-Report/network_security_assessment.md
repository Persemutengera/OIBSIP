# Full Network Security Assessment Report

Project: Full Network Security Assessment
Assessment Type: Local Laboratory Network Assessment
Date: 01 September 2026
Tester: Perseverance Mutengera
Environment: Kali Linux / VirtualBox Laboratory
Status: Completed

## 1. Executive Summary

A structured security assessment was performed against an isolated local test network to identify exposed hosts, network services, web-server weaknesses, and potentially sensitive information observable through network traffic.

The assessment used Nmap, Wireshark and Nikto. Testing was limited to the authorized laboratory environment.

### Overall Security Posture:

[LOW / MODERATE / HIGH]

The assessment identified [NUMBER] findings, consisting of:

Critical: [NUMBER]
High: [NUMBER]
Medium: [NUMBER]
Low: [NUMBER]
Informational: [NUMBER]

The most significant finding was [FINDING], which presents a potential risk of [IMPACT].

Priority remediation actions include:

[RECOMMENDATION]
[RECOMMENDATION]
[RECOMMENDATION]

## 2. Assessment Scope
### 2.1 Target Network

IP Range: 127.0.0.1

2.2 Included Assets

The assessment included:

Hosts identified within the authorized laboratory network
TCP/UDP services exposed by those hosts
HTTP services where present
DNS traffic
ARP traffic
Network traffic generated during the assessment

## 2.3 Assessment Window

Start: 05 September 2026

End: 06 September 2026

Timezone: SAST (UTC+2)

## 2.4 Out of Scope

The following activities were excluded:

- Internet-facing systems
- Third-party systems
- Denial-of-service testing
- Destructive testing
- Persistence
- Data destruction
- Unauthorized access
- Testing outside the defined laboratory network
  
## 3. Methodology

The assessment followed a structured approach based on established security-testing methodologies.

The OWASP Web Security Testing Guide was used as a reference for web-security testing and reporting practices.

PTES was used as a reference for structured penetration-testing phases, scope definition, technical testing and reporting.

CVSS was used as the primary reference for consistent vulnerability severity assessment.

The assessment consisted of:

1. Reconnaissance
2. Network traffic analysis
3. Web-server vulnerability assessment
4. Findings analysis
5. Risk classification
6. Remediation planning
   
## 4. Tools Used
|Tool	               |Purpose                                                    |
|--------------------|-----------------------------------------------------------|
|Nmap	               | Host discovery, port scanning and service identification  |
|Wireshark	         | Network traffic capture and protocol analysis             |
|Nikto	             | Web-server security assessment                            |
|Kali Linux	         | Assessment platform                                       |
|VirtualBox	         | Isolated laboratory environment                           |

## 5. Phase 1 — Reconnaissance
### 5.1 Objective

The objective of this phase was to identify active hosts, open ports, exposed services, service versions and operating-system information within the authorized network.

5.2 Commands Used
sudo nmap -sn 127.0.0.1
sudo nmap -sV -O 127.0.0.1 -oN nmap_results.txt
5.3 Discovered Hosts
IP Address	Status	Operating System	Notes
[IP]	Up	[OS]	[Notes]
[IP]	Up	[OS]	[Notes]
5.4 Open Ports and Services
Host	Port	Protocol	Service	Version
[IP]	[PORT]	TCP	[SERVICE]	[VERSION]
[IP]	[PORT]	TCP	[SERVICE]	[VERSION]

### 5.5 Evidence




The complete raw Nmap output is provided in:

nmap_results.txt

6. Phase 2 — Network Traffic Analysis
6.1 Objective

The objective was to capture and analyse network traffic for at least five minutes and investigate HTTP, DNS and ARP protocols.

6.2 Capture Details

Capture Duration: 8 minutes

Interface: [INTERFACE]

Capture File: wireshark_capture.pcap

6.3 HTTP Analysis

Wireshark display filter:

http

Additional filter:

http.request.method == "POST"
Observations

[DOCUMENT YOUR ACTUAL OBSERVATIONS]

Sensitive Information

[STATE WHETHER SENSITIVE INFORMATION WAS OBSERVED]

If sensitive information was observed, describe the type without exposing the actual value.

Security Impact

[DESCRIBE IMPACT]

Evidence




6.4 DNS Analysis

Wireshark display filter:

dns
Observations

[DOCUMENT ACTUAL DNS ACTIVITY]

Security Assessment

[STATE WHETHER ANY SUSPICIOUS ACTIVITY WAS IDENTIFIED]

Evidence




## 6.5 ARP Analysis

Wireshark display filter:

arp
Observations

No activity

Security Assessment

No anomalies were observed

Evidence




## 7. Phase 3 — Web Vulnerability Assessment
7.1 Objective

Where an HTTP service was identified during reconnaissance, Nikto was used to assess the web server for common configuration weaknesses and information disclosure.

7.2 Command
nikto -h http://127.0.0.1
7.3 Results
Finding	Description	Severity
[Finding]	[Description]	[Severity]
[Finding]	[Description]	[Severity]

7.4 Evidence




## 8. Findings Register
Finding ID	Description	Severity	Affected Asset	Recommended Fix
F-001	[Finding]	[Severity]	[Asset]	[Fix]
F-002	[Finding]	[Severity]	[Asset]	[Fix]
F-003	[Finding]	[Severity]	[Asset]	[Fix]

## 9. Detailed Findings
F-001 — [Finding Name]

Severity: [Critical/High/Medium/Low/Info]

Affected Asset: [Asset]

CVSS: [Score if applicable]

Description

[Explain the vulnerability or security weakness.]

Evidence

[Describe the evidence obtained from Nmap, Wireshark or Nikto.]

Impact

[Explain what could happen if the weakness were abused.]

Recommendation

[Provide a specific remediation.]

Remediation Priority

Priority: [P1/P2/P3/P4]

Estimated Effort: [Easy/Medium/Hard]

F-002 — [Finding Name]

Severity: [Severity]

Affected Asset: [Asset]

CVSS: [Score if applicable]

Description

[Description]

Evidence

[Evidence]

Impact

[Impact]

Recommendation

[Recommendation]

Remediation Priority

Priority: [P1/P2/P3/P4]

Estimated Effort: [Easy/Medium/Hard]

## 10. Remediation Roadmap
Priority	Finding	Recommended Action	Effort	Target
P1	[Critical/High finding]	[Action]	Hard	Immediate
P1	[High finding]	[Action]	Medium	Immediate
P2	[Medium finding]	[Action]	Medium	30 days
P3	[Low finding]	[Action]	Easy	60–90 days
P4	Informational	Monitor/improve	Easy	Ongoing

## Recommended Order

1. Critical findings

Address immediately because they may represent significant compromise risk.

2. High findings

Address next, particularly weaknesses that expose credentials, administrative interfaces or sensitive information.

3. Medium findings

Address after critical and high-risk issues.

4. Low findings

Address as part of security hardening.

5. Informational observations

Track them as part of continuous security monitoring.

## 11. Conclusion

The assessment successfully evaluated the security posture of the authorized laboratory network using network reconnaissance, traffic analysis and web-server assessment techniques.

Nmap identified the hosts and services exposed within the assessment scope. Wireshark provided visibility into HTTP, DNS and ARP traffic. Where an HTTP service was present, Nikto was used to identify potential web-server configuration and security weaknesses.

The assessment demonstrates the importance of reducing unnecessary network exposure, protecting sensitive communications with encryption, maintaining secure server configurations and continuously monitoring network activity.

The recommended remediation roadmap should be implemented according to the severity and business impact of each finding, with Critical and High findings receiving the highest priority.

## 12. References
OWASP Web Security Testing Guide: https://owasp.org/www-project-web-security-testing-guide/
PTES: https://www.pentest-standard.org/
PTES Technical Guidelines: https://www.pentest-standard.org/index.php/PTES_Technical_Guidelines
FIRST CVSS: https://www.first.org/cvss/
Nmap Documentation: https://nmap.org/docs.html
Wireshark Documentation: https://www.wireshark.org/docs/
Nikto Documentation: https://github.com/sullo/nikto

## 13. Evidence Files

The following evidence files accompany this report:

network_security_assessment.md
nmap_results.txt
wireshark_capture.pcap
screenshots/nmap_scan.png
screenshots/wireshark_http.png
screenshots/wireshark_dns.png
screenshots/wireshark_arp.png
screenshots/nikto_scan.png
