Comprehensive Internal Network and Web Application Penetration Test


**Table of Contents**
Project Overview
Network Topology
Tools and Technologies
Configuration Steps
Results and Findings
Author

**Project Overview**

The purpose of this project was to conduct a penetration test against a legacy Linux server and its associated web application, building on a previous vulnerability assessment completed on 31 August 2026.

The engagement aimed to determine whether previously identified vulnerabilities could be exploited, establish the level of access they could provide, assess the potential business impact, and recommend appropriate security improvements.

The assessment focused on:
Validating critical vulnerabilities identified during the earlier vulnerability assessment.
Assessing the security of exposed internal network services.
Evaluating web application security.
Determining the level of access obtainable through confirmed vulnerabilities.
Identifying potential exposure of sensitive information and user credentials.
Prioritising remediation based on technical severity and business impact.

Testing was conducted within an isolated VirtualBox lab environment. Denial-of-service attacks, destructive payloads, and testing against systems outside the authorised lab were excluded.

**Network Topology**

The assessment used an isolated virtual network containing an attacker machine and a deliberately vulnerable target environment.

**Lab Environment**

Attacker Machine: Kali Linux 2026.1
Attacker IP Address: 192.168.10.3
Target Machine: Metasploitable 2
Target IP Address: 192.168.10.4
Web Application: Damn Vulnerable Web Application (DVWA)
Virtualisation Platform: Oracle VirtualBox

**Network and Application Components**

The target environment hosted several vulnerable services and applications, including:
FTP
SSH
SMB
MySQL
UnrealIRCd
VNC
A vulnerable bind shell service
DVWA web application
Mutillidae web application

**Key Services Examined**
TCP/6667: UnrealIRCd service
TCP/1524: Unauthenticated bind shell
TCP/5900: VNC service
Web application: DVWA, including SQL Injection and CSRF testing areas

The network assessment focused on the interaction between the Kali Linux attacker machine and the Metasploitable 2 target. Web application testing was performed against DVWA within the same controlled environment.

**Tools and Technologies**

Kali Linux 2026.1 — Attacker operating system and penetration testing environment.
Metasploitable 2 — Intentionally vulnerable Linux target used for controlled security testing.
Oracle VirtualBox — Virtualisation platform used to create the isolated lab environment.
Nmap — Network discovery and service identification.
Nessus — Vulnerability scanning and validation reference from the previous assessment.
Metasploit Framework — Controlled validation of selected network vulnerabilities.
Burp Suite Community Edition — Web application traffic inspection and request analysis.
DVWA — Deliberately vulnerable web application used to assess common web security weaknesses.
Mutillidae — Additional vulnerable web application included in the lab environment.
MySQL — Database service examined during web application testing.
Wireshark — Network traffic analysis tool included in the broader networking and security toolkit.

**Configuration Steps**

**Define the engagement scope**

Identify the authorised target systems and testing boundaries.
Establish the target server as 192.168.10.4.
Confirm that testing is restricted to the isolated lab environment.
Exclude denial-of-service attacks, destructive payloads, and unauthorised external systems.

**Prepare the virtual lab**

Set up Kali Linux and Metasploitable 2 in Oracle VirtualBox.
Configure the virtual network.
Assign the attacker address 192.168.10.3 and target address 192.168.10.4.
Confirm connectivity between both systems before testing.

**Review the previous vulnerability assessment**

Examine the earlier Nmap and Nessus results.
Review the critical and high-priority findings.
Select relevant vulnerabilities for controlled validation.
Re-verify that selected findings remain present before testing.

**Conduct internal network penetration testing**

Use Metasploit to assess the selected network-service vulnerabilities.
Examine the UnrealIRCd backdoor finding on TCP port 6667.
Assess the unauthenticated bind shell finding on TCP port 1524.
Record the resulting access level and supporting evidence.

**Conduct web application penetration testing**

Access DVWA within the lab environment.
Set the application security level to Low, as specified in the engagement.
Assess the SQL Injection vulnerability in the user ID input field.
Examine the password-change functionality for Cross-Site Request Forgery (CSRF).
Use Burp Suite Community Edition to inspect and review HTTP requests.

**Assess the impact of confirmed vulnerabilities**

Determine the access level associated with each finding.
Review the potential exposure of database records and user credentials.
Assess the potential for unauthorised account changes.
Consider the operational and reputational consequences for the organisation.

**Prioritise security risks**

Classify findings according to severity and potential business impact.
Distinguish between network-level and web application vulnerabilities.
Identify findings requiring immediate containment and remediation.

**Develop remediation recommendations**

Recommend removing unauthorised backdoors and vulnerable services.
Recommend replacing unsupported operating systems.
Address insecure database queries using parameterised statements.
Introduce CSRF protection for state-changing web requests.
Review authentication, credential management, and access controls.

**Document the engagement**

Record the methodology, findings, and evidence.
Summarise the exploitation results.
Prepare a risk prioritisation matrix.
Provide a remediation plan with immediate, short-term, and medium-term actions.

**Results and Findings**

The engagement successfully validated four vulnerabilities: two affecting internal network services and two affecting the DVWA web application.
The results demonstrated that several weaknesses previously identified during vulnerability scanning were exploitable within the controlled test environment.

1. UnrealIRCd Backdoor

Category: Network service vulnerability
Port: TCP/6667
Reference: CVE-2010-2075 / Nessus Plugin 46882
Severity: Critical
Result: Successful access to an interactive operating system shell with root privileges.
Security implication: The affected service provided a direct route to complete system compromise.

2. Unauthenticated Bind Shell

Category: Network service vulnerability
Port: TCP/1524
Reference: Nessus Plugin 51988
Severity: Critical
Result: Successful access to an interactive shell with root privileges without authentication.
Security implication: An exposed service provided direct administrative-level access, representing a serious security control failure.

3. SQL Injection

Category: Web application vulnerability
OWASP Reference: A03:2021 – Injection
Location: DVWA SQL Injection user ID field
Severity: Critical
Result: Successful access to the application's user credential table, including usernames and MD5 password hashes.
Security implication: Improper handling of user input exposed sensitive database information.

4. Cross-Site Request Forgery (CSRF)

Category: Web application vulnerability
OWASP Reference: A01:2021 – Broken Access Control (as classified in the report)
Location: DVWA password-change functionality
Severity: High
Result: The assessment demonstrated that a password-change request could be submitted without an anti-CSRF token or an additional origin validation control.
Security implication: An authenticated user's session could potentially be abused to make unauthorised account changes, including administrator account changes.

**Overall Findings**

Finding: UnrealIRCd Backdoor
Severity: Critical
Confirmed Result: Root-level system access

Finding: Unauthenticated Bind Shell
Severity:Critical
Confirmed Result: Root-level system access

Finding: SQL Injection
Severity: Critical
Confirmed Result: Database credential exposure

Finding: CSRF
Severity:High
Confirmed Result: Unauthorised account-change potential

The assessment established that both network-level vulnerabilities provided immediate root access, while the SQL Injection and CSRF findings exposed significant web application and account security weaknesses.

**Business Impact**

The findings presented several potential risks to the organisation:

Data confidentiality: Exposure of user credentials and sensitive application data.
System integrity: Potential unauthorised modification of system and application information.
Service availability: Root-level access could enable disruption of services.
Account security: CSRF could expose authenticated users to unauthorised account changes.
Business reputation: A compromise involving internal applications could undermine staff and stakeholder confidence.

**Recommendations and Remediation**

Immediate actions (0–72 hours)

Isolate the affected server from untrusted network paths.
Remove the unauthenticated bind shell and investigate its origin.
Remove the affected UnrealIRCd installation and replace it with a trusted version if required.
Remediate the SQL Injection vulnerability using parameterised queries or prepared statements.
Treat the host as potentially compromised while investigating the findings.

Short-term actions (7–30 days)

Implement CSRF tokens for all state-changing requests.
Require appropriate re-authentication for sensitive account changes.
Rotate credentials that may have been exposed.
Review access controls and authentication mechanisms.
Plan migration away from the unsupported Ubuntu 8.04 operating system.
Medium-term actions (30–90 days)
Complete the operating system migration or rebuild.
Repeat vulnerability scanning and penetration testing after remediation.
Introduce secure software development lifecycle checks.
Make secure input handling and parameterised database queries part of code review.
Establish a recurring penetration testing programme.

**Key Learning Outcomes**

This project strengthened my understanding of how vulnerability assessment findings can be validated through controlled penetration testing. It also reinforced the importance of combining technical testing with evidence-based analysis, business risk assessment, and clear remediation planning.

Ethical Note: All testing was performed in an isolated virtual lab using intentionally vulnerable systems. No production systems or unauthorised external targets were tested.

Author: Olajumoke Idunnuola Olaniyan

Role: Lead VAPT Analyst



Focus Areas: Networking, Vulnerability Assessment, Penetration Testing, Network Security, and Web Application Security

Contact: Add your preferred professional email address or LinkedIn profile here.
