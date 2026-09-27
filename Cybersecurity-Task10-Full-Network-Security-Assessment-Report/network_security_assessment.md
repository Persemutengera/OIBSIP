# Network Security Assessment Report
## Task 10 – Full Network Security Assessment

Assessment Type: Local Network Security Assessment
Environment: Isolated/Test Network
Tools Used: Nmap, Wireshark, Nikto
Report Format: Markdown
Assessment Date: September 2026

## 1. Executive Summary

This report documents a structured security assessment of a local test network and web service. The assessment was conducted in a controlled laboratory environment using network discovery, service enumeration, packet capture, and web-server vulnerability scanning.

The assessment used the following tools:

- Nmap – network discovery, port scanning, and service enumeration
- Wireshark – network traffic capture and protocol analysis
- Nikto – web-server configuration and security checks
- Markdown – documentation and reporting

The purpose of the assessment was to identify exposed services, observe network communication, and identify potential security weaknesses in the test environment.

The results demonstrated the importance of reducing unnecessary exposed services, securing HTTP communications, implementing appropriate HTTP security headers, and monitoring network traffic.

Important: All testing was performed against systems in a controlled laboratory environment for educational and security-assessment purposes.

## 2. Scope of Assessment
### 2.1 Assessment Objectives

The objectives of the assessment were to:

1. Identify active hosts on the test network.
2. Identify open TCP ports.
3. Determine services running on discovered ports.
4. Capture and analyse network traffic.
5. Assess the security configuration of the local web server.
6. Identify potentially missing HTTP security controls.
7. Document findings and provide remediation recommendations.
   
### 2.2 Network Scope

The assessment was restricted to the local laboratory environment.

## Systems
|System	                  | Address	                     | Purpose                    |
|-------------------------|------------------------------|----------------------------|
|Kali Linux	              | 10.0.2.15/24	               | Security testing machine   |
|Test/DVWA environment	  | Local lab target	           | Vulnerable web application |
|Web test server	        | 127.0.0.1	                   | Local HTTP testing         |

The exact IP addresses discovered during the Nmap assessment should be recorded in the final evidence section where applicable.

## 3. Rules of Engagement

The following rules were followed during the assessment:

- Testing was performed only against authorised laboratory systems.
- No external public systems were targeted.
- No denial-of-service testing was performed.
- Vulnerability exploitation was limited to controlled demonstrations.
- Captured traffic was generated from the laboratory environment.
- Results were documented for educational and defensive security purposes.
  
## 4. Assessment Methodology

The assessment followed these stages:

Scope Definition
      |
      v
Network Discovery
      |
      v
Port and Service Enumeration
      |
      v
Web Server Assessment
      |
      v
Network Traffic Capture
      |
      v
Analysis of Findings
      |
      v
Risk Assessment
      |
      v
Remediation Recommendations
      |
      v
Final Report

## 5. Tools Used
### 5.1 Nmap

Nmap was used to identify hosts, open ports, and services running on the target system.

Example command:

nmap -sV 127.0.0.1

A more detailed scan can be performed using:

nmap -sC -sV 127.0.0.1
Purpose

Nmap was used to:

- Discover open ports
- Identify running services
- Determine service versions
- Identify potentially unnecessary exposed services
- Provide information for further security analysis
  
### 5.2 Wireshark

Wireshark was used to capture and analyse network traffic generated during testing.

The capture was performed on the appropriate network interface.

The following traffic was investigated:

- HTTP
- DNS
- TCP
- UDP
- Loopback traffic where applicable

Example display filters:

http
dns
tcp
ip.addr == 127.0.0.1

### 5.3 Nikto

Nikto was used to assess the security configuration of the HTTP server.

Example:

nikto -h http://127.0.0.1

Nikto checks for issues such as:

- Missing security headers
- Dangerous HTTP methods
- Default files
- Server configuration weaknesses
- Potentially exposed resources
- Known web-server configuration problems
  
## 6. Network Discovery

The first stage of the assessment involved identifying the target host and determining whether it was reachable.

A basic connectivity test can be performed using:

ping 127.0.0.1

Nmap was then used for port and service discovery.

Example:

nmap -sV 127.0.0.1

## Objective

The purpose of this stage was to establish:

- Whether the host was reachable
- Which TCP ports were open
- Which services were accessible
- Which services required further investigation
  
## 7. Nmap Results

The Nmap scan identified services exposed by the target system.

The discovered ports and services should be recorded below based on the final scan output:

|Port	   | Protocol	  |Service	       |Version	                               |Security Consideration     |
|--------|-------------|----------------|-----------------------------------------|---------------------------|
|80 	   | TCP	        |HTTP	          |Apache httpd 2.4.68 (Debian)	          |Review whether required    |
|443	   | TCP	        |HTTPS/SSL HTTP	 |Apache httpd 2.4.68 (Debian)	          |Restrict if unnecessary    |
|8000	   | TCP	        |HTTP	          |SimpleHTTPServer 0.6 (Python 3.13.12)	 |Ensure securely configured |

## Analysis

Open ports represent potential attack surfaces. A service does not necessarily represent a vulnerability simply because it is exposed; however, every exposed service should be justified, securely configured, patched, and monitored.

Unnecessary services should be disabled or restricted through firewall rules.

## 8. Web Server Assessment

The local HTTP server was tested as part of the assessment.

The test server was observed running on:

127.0.0.1:80

Nikto was used to identify potential HTTP configuration issues.

The assessment identified the following observations.

### 8.1 Missing Strict-Transport-Security Header

Nikto reported:

Suggested security header missing:
strict-transport-security

### Security Impact

The HTTP Strict Transport Security (HSTS) header instructs compatible browsers to use HTTPS instead of HTTP for future connections.

Its absence does not automatically mean that the server is vulnerable, particularly when the test server is operating only over HTTP in a laboratory environment. However, production websites using HTTPS should consider implementing HSTS.

### Recommendation

For a production HTTPS service, configure the web server to return an appropriate HSTS header.

Example:

Strict-Transport-Security: max-age=31536000; includeSubDomains

This should only be deployed when HTTPS is correctly configured and intended for the domain.

## 9. HTTP Methods

Nikto reported the following allowed HTTP methods:

OPTIONS
HEAD
GET
POST

### Observation

The server responded as allowing:

- OPTIONS
- HEAD
- GET
- POST

These methods are commonly used by web applications.

## Security Consideration

HTTP methods should be restricted to those required by the application. Unnecessary methods should be disabled where possible.

For example, an application that does not require a particular method should not expose it unnecessarily.

## 10. Wireshark Traffic Analysis

Wireshark was used to capture traffic generated during the assessment.

The capture allowed network communication to be observed at the packet level.

### 10.1 HTTP Traffic

HTTP traffic was successfully generated during testing of the local web server.

The traffic could be observed using the Wireshark display filter:

http

HTTP packets showed communication between the client and the local web server.

The test environment therefore demonstrated how HTTP requests and responses can be observed during packet capture.

### 10.2 DNS Traffic

DNS traffic was investigated using:

dns

If no DNS packets appear during a particular capture, this does not necessarily indicate a Wireshark problem.

For example, when accessing a service through:

127.0.0.1

DNS resolution is not required because the address is already specified as a loopback IP address.

Therefore, a DNS filter can legitimately return no packets during a localhost-only HTTP test.

### 10.3 Loopback Traffic

When testing:

127.0.0.1

the traffic is generated through the loopback interface rather than the normal Ethernet/Wi-Fi interface.

Therefore, the correct interface must be selected in Wireshark when capturing localhost traffic.

The loopback interface can be verified on Kali Linux using:

ip addr

The interface commonly appears as:

lo

## 11. Network Traffic Findings

The Wireshark assessment demonstrated that packet capture can provide visibility into communications occurring within the test environment.

The following protocols were relevant to the assessment:

|Protocol	         |Observation	                      |Security Relevance                          |
|--------------------|------------------------------------|--------------------------------------------|
|HTTP	               |Web traffic observed	             |Data is transmitted without TLS encryption  |
|TCP	               |Transport traffic observed	       |Provides connection-level communication     | 
|DNS	               |Dependent on hostname resolution	 |Can reveal queried domains                  | 
|Loopback	         |Used for localhost traffic	       |Required for localhost packet capture       |

## 12. Security Findings

The following findings were identified during the assessment.

ID	   Finding	             Evidence	Risk
F-01	HSTS header missing	 Nikto	   Medium
F-02	HTTP service exposed	Web-server testing	Medium
F-03	HTTP methods available	Nikto	Low/Medium
F-04	Network services exposed	Nmap	Depends on service
F-05	Unencrypted HTTP traffic observable	Wireshark	Medium

Risk levels are indicative for this laboratory assessment. Actual production risk depends on the system architecture, exposure, data handled, authentication controls, and compensating security measures.

## 13. Finding F-01 – Missing HSTS
Description

Nikto identified the absence of the:

Strict-Transport-Security

HTTP response header.

Evidence
+ Target Host: 127.0.0.1
+ Target Port: 80
+ Suggested security header missing:
  strict-transport-security
Potential Impact

Without HSTS, browsers are not instructed to automatically enforce HTTPS for the applicable domain.

Recommendation

For production HTTPS services:

Enable HTTPS.
Install and correctly configure a valid TLS certificate.
Configure HSTS.
Test the configuration before deployment.
14. Finding F-02 – HTTP Service
Description

The assessment identified an HTTP service running on the local test server.

Potential Impact

HTTP does not provide transport encryption.

Traffic sent using ordinary HTTP can potentially be viewed or modified by an attacker who is able to intercept the network traffic.

Recommendation

Production applications should use HTTPS/TLS.

Example:

HTTP
  |
  | Replace with
  v
HTTPS
15. Finding F-03 – HTTP Methods
Description

Nikto identified the following allowed methods:

OPTIONS
HEAD
GET
POST
Potential Impact

HTTP methods that are not required by the application can increase the exposed attack surface.

Recommendation

The web server should allow only the HTTP methods required by the application.

The configuration should be reviewed periodically to ensure unnecessary methods are disabled.

16. Finding F-04 – Exposed Network Services
Description

Nmap was used to identify accessible network services.

Potential Impact

Each exposed service provides a potential entry point that must be securely configured and maintained.

Recommendation

Administrators should:

Disable unnecessary services.
Restrict administrative services to trusted networks.
Use firewall rules.
Keep services patched.
Monitor exposed ports.
Regularly perform vulnerability assessments.
17. Finding F-05 – Unencrypted HTTP Traffic
Description

Wireshark demonstrated that HTTP communication could be captured and inspected.

Potential Impact

Because ordinary HTTP does not encrypt application-layer data, sensitive information transmitted through HTTP may be visible to someone capable of capturing the traffic.

Recommendation

Use HTTPS/TLS for production web applications.

Sensitive information such as:

Passwords
Session identifiers
Personal information
Authentication tokens

should not be transmitted through unencrypted HTTP.

18. Risk Assessment

The findings can be prioritised based on their potential impact and likelihood.

Finding	Impact	Likelihood	Priority
Missing HSTS	Medium	Medium	Medium
HTTP service	Medium	Medium	Medium
HTTP methods	Low–Medium	Medium	Medium
Exposed services	Depends on service	Depends on exposure	Review
Unencrypted HTTP	Medium–High for sensitive data	Medium	High for production

These priorities are intended for the controlled laboratory environment and should not be treated as a formal enterprise risk rating without additional environmental information.

19. Remediation Recommendations
19.1 Use HTTPS

Configure production web applications to use HTTPS instead of plain HTTP.

Client
   |
   | HTTPS/TLS
   v
Web Server
19.2 Configure Security Headers

Consider implementing appropriate security headers such as:

Strict-Transport-Security
Content-Security-Policy
X-Content-Type-Options
Referrer-Policy

The exact configuration should be tested against the application's requirements.

19.3 Restrict Network Services

Only required services should be exposed.

Example firewall approach:

sudo ufw status

Unnecessary ports should be closed or restricted.

19.4 Patch Services

Services discovered through Nmap should be kept updated.

Administrators should regularly check:

Operating-system updates
Web-server updates
Application updates
Database updates
Security patches
19.5 Monitor Network Traffic

Network monitoring can help identify:

Unexpected connections
Unusual protocols
Repeated connection attempts
Suspicious DNS requests
Unexpected external communication

Wireshark can be used for detailed investigation, while production environments should normally use dedicated monitoring and detection solutions.

20. Evidence Collected

The following evidence should be included with the assessment:

evidence/
├── nmap_scan.txt
├── nikto_results.txt
├── wireshark_http_capture.pcapng
├── screenshots/
│   ├── nmap-results.png
│   ├── nikto-results.png
│   ├── wireshark-http.png
│   └── wireshark-dns.png
└── NETWORK_SECURITY_ASSESSMENT.md
21. Limitations

The assessment was performed in a controlled laboratory environment.

The following limitations apply:

The assessment did not represent a full production penetration test.
Only authorised laboratory systems were tested.
Internet-facing attack scenarios were not assessed.
Denial-of-service testing was not performed.
The assessment did not include a complete source-code review.
Risk ratings may differ in a production environment.
Results depend on the configuration of the test environment at the time of testing.
22. Conclusion

The network security assessment demonstrated the use of multiple security tools to identify and analyse network and web-server security issues.

Nmap provided information about network services and exposed ports. Wireshark provided packet-level visibility into network communication, including HTTP traffic. Nikto identified web-server configuration observations, including the absence of the HSTS security header and the HTTP methods supported by the test server.

The assessment demonstrates the importance of:

Minimising exposed services
Using HTTPS/TLS
Configuring appropriate security headers
Restricting unnecessary HTTP methods
Applying security patches
Monitoring network traffic
Regularly reassessing systems for security weaknesses

The identified issues should be addressed according to the risk and purpose of the environment. For production systems, additional controls such as firewall segmentation, vulnerability management, secure configuration management, logging, monitoring, and continuous security testing should also be considered.

23. Appendix A – Useful Commands
Nmap
nmap <TARGET_IP>
nmap -sV <TARGET_IP>
nmap -sC -sV <TARGET_IP>
Nikto
nikto -h http://127.0.0.1

Save results:

nikto -h http://127.0.0.1 -output nikto_results.txt
Network Interfaces
ip addr
Check Listening Services
ss -tulnp
UFW
sudo ufw status
24. Appendix B – Assessment Checklist

Assessment scope defined

Target environment identified

Nmap used for network/service discovery

Web server assessed with Nikto

HTTP traffic captured with Wireshark

DNS traffic investigated

Security findings documented

Risks discussed

Remediation recommendations provided

Assessment limitations documented

Evidence identified

Final report prepared

Assessment Status

Assessment completed: September 2026

Primary tools:

Nmap
Wireshark
Nikto
Markdown

Environment:

Controlled Local Laboratory
