# Appendix: Templates and Checklists

## Overview

Practical templates and checklists for conducting organizational threat modeling. Copy, modify, and use these for your engagements.

## A. Reconnaissance Checklist

### Pre-Engagement

```
[ ] Statement of work signed
[ ] Scope document finalized
[ ] Authorization letter obtained
[ ] Contact information documented
[ ] Tools prepared and tested
[ ] Storage for findings set up
[ ] Timeline confirmed
```

### Domain Enumeration

```
[ ] Primary domain identified
[ ] Alternate TLDs checked (.com, .net, .org, .io, etc.)
[ ] Subsidiary domains researched
[ ] Historical domains checked (WHOIS history)
[ ] Certificate transparency logs searched
[ ] Subdomain enumeration (subfinder, amass, assetfinder)
[ ] DNS brute force (if needed)
[ ] Zone transfer attempts
[ ] All results deduplicated and organized
```

### Infrastructure Mapping

```
[ ] DNS resolution for all domains
[ ] IP addresses documented
[ ] ASN and netblock identification
[ ] Cloud provider identification (AWS, Azure, GCP)
[ ] Reverse DNS lookups
[ ] Port scanning (common ports)
[ ] Service fingerprinting
[ ] SSL/TLS configuration checked
[ ] CDN usage identified
```

### Technology Stack

```
[ ] Web server identification
[ ] Application framework detection
[ ] Programming languages identified
[ ] CMS/platform detection (WordPress, Drupal, etc.)
[ ] JavaScript frameworks (React, Angular, Vue)
[ ] Backend technology indicators
[ ] Database type (if detectable)
[ ] Mail infrastructure (MX records, mail server software)
[ ] DNS provider identified
[ ] SPF/DKIM/DMARC records checked
```

### Code Repositories

```
[ ] GitHub organization found
[ ] GitLab groups searched
[ ] Bitbucket workspaces checked
[ ] Public repositories enumerated
[ ] Employee personal repos searched
[ ] Gists and code snippets searched
[ ] Package registries checked (npm, PyPI, Docker Hub)
[ ] Sensitive data scanning (API keys, credentials)
[ ] Repository metadata documented
```

### Cloud Resources

```
[ ] S3 bucket enumeration
[ ] S3 bucket permissions tested
[ ] Azure blob storage checked
[ ] Google Cloud Storage checked
[ ] Cloud function endpoints identified
[ ] API Gateway endpoints found
[ ] Misconfigured resources documented
```

### OSINT

```
[ ] LinkedIn employee enumeration
[ ] Organizational structure mapped
[ ] Job postings analyzed
[ ] Social media accounts identified
[ ] Technical blogs found and reviewed
[ ] Conference talks and presentations searched
[ ] News articles and press releases reviewed
[ ] Breach databases searched
[ ] Pastebin and paste sites searched
[ ] Dark web mentions checked (if applicable)
```

### Third-Party Services

```
[ ] JavaScript third-party includes identified
[ ] Privacy policy reviewed for vendors
[ ] SaaS login pages tested
[ ] OAuth integrations identified
[ ] Payment processor identified
[ ] Email service provider confirmed
[ ] Analytics and tracking identified
[ ] CDN and security services documented
```

### Documentation

```
[ ] All findings documented in real-time
[ ] Screenshots captured for key findings
[ ] Tool output saved (raw files)
[ ] Asset inventory populated
[ ] Discovery methods noted for each asset
[ ] Interesting findings flagged for deeper analysis
[ ] Critical findings immediately reported
```

## B. Asset Inventory Template

### Spreadsheet Structure

**Column Headers:**
```
Asset ID
Asset Type [Domain, Subdomain, IP, Web App, API, Database, Repository, Service, Employee]
Asset Name
Description
Owner/Department
Discovery Method
Discovery Date
Status [Active, Inactive, Deprecated, Unknown]
Technology Stack
Services/Ports
Cloud Provider
Region/Location
IP Address
DNS Records
SSL Certificate Info
Criticality [Critical, High, Medium, Low]
Data Sensitivity [Restricted, Confidential, Internal, Public]
Business Impact
Compliance Scope [PCI, HIPAA, SOC2, GDPR, etc.]
Dependencies (depends on)
Dependents (depended on by)
Notes
Last Verified
```

**Example Entry:**
```
Asset ID: A001
Asset Type: Web Application
Asset Name: app.example.com
Description: Customer portal for account management
Owner: Engineering - Product Team
Discovery Method: Known primary domain
Discovery Date: 2025-12-20
Status: Active
Technology Stack: React (frontend), Node.js/Express (backend), PostgreSQL, Redis
Services/Ports: HTTPS/443, HTTP/80 (redirect)
Cloud Provider: AWS
Region/Location: us-east-1
IP Address: 54.210.123.45 (ALB)
DNS Records: CNAME to lb-prod.us-east-1.elb.amazonaws.com
SSL Certificate Info: Let's Encrypt, expires 2026-03-15
Criticality: Critical
Data Sensitivity: Confidential (Customer PII)
Business Impact: Revenue-generating (primary customer interface)
Compliance Scope: PCI-DSS, SOC2, GDPR
Dependencies: A023 (database), A112 (auth service)
Dependents: A003 (mobile app), A156 (admin dashboard)
Notes: Primary production environment, 24/7 availability required
Last Verified: 2025-12-23
```

## C. Threat Scenario Template

```
THREAT SCENARIO: [Scenario Name]

Threat ID: TM-YYYY-NNN
Date Created: YYYY-MM-DD
Created By: [Name]
Last Updated: YYYY-MM-DD

ASSETS AFFECTED:
- [Asset ID]: [Asset Name]
- [Asset ID]: [Asset Name]

THREAT ACTOR:
Type: [Opportunistic, Criminal, Nation-State, Hacktivist, Insider, etc.]
Motivation: [Financial, Espionage, Disruption, etc.]
Capability: [Low, Medium, High, Advanced]
Resources: [Minimal, Moderate, Significant, Unlimited]

ATTACK VECTOR:
Entry Point: [Describe how attacker gains initial access]
Prerequisites:
- [Condition or requirement]
- [Condition or requirement]

ATTACK STEPS:
1. [Reconnaissance / Initial Access]
2. [Exploitation / Execution]
3. [Persistence / Privilege Escalation]
4. [Lateral Movement]
5. [Collection / Exfiltration]
6. [Impact / Objectives]

TECHNICAL DETAILS:
Tools/Techniques: [Specific tools, exploits, or methods]
MITRE ATT&CK Mapping:
- [Tactic]: [Technique ID] - [Technique Name]
- [Tactic]: [Technique ID] - [Technique Name]

IMPACT ANALYSIS:
Financial Impact:
- Direct costs: $[amount]
- Indirect costs: $[amount]
- Total: $[amount]

Operational Impact:
- [Description of business disruption]
- Downtime estimate: [hours/days]
- Affected systems: [list]

Compliance Impact:
- Regulations violated: [GDPR, PCI, HIPAA, etc.]
- Potential fines: $[amount]
- Notification requirements: [Yes/No, scope]

Reputation Impact:
- Customer trust: [High/Medium/Low impact]
- Media coverage: [Expected level]
- Recovery time: [estimate]

Data Impact:
- Data types: [PII, Payment, Health, Proprietary, etc.]
- Records affected: [number]
- Sensitivity: [Restricted/Confidential/Internal/Public]

RISK ASSESSMENT:
Likelihood: [1-5] - [Very Low/Low/Medium/High/Very High]
Likelihood Justification:
- [Factor 1]
- [Factor 2]

Impact: [1-5] - [Very Low/Low/Medium/High/Very High]
Impact Justification:
- [Factor 1]
- [Factor 2]

Inherent Risk: [Low/Medium/High/Critical]

CURRENT CONTROLS:
Control 1:
- Description: [What control exists]
- Effectiveness: [Low/Medium/High]
- Gaps: [Identified weaknesses]

Control 2:
- Description: [What control exists]
- Effectiveness: [Low/Medium/High]
- Gaps: [Identified weaknesses]

Residual Risk After Current Controls: [Low/Medium/High/Critical]

INDICATORS OF COMPROMISE:
- [Indicator 1]
- [Indicator 2]
- [Indicator 3]

DETECTION METHODS:
- [How to detect this attack]
- [Monitoring/logging sources]
- [Alert signatures]

RECOMMENDED MITIGATIONS:
Priority 1 (Immediate):
- [Mitigation 1]
  - Cost: $[amount]
  - Timeline: [timeframe]
  - Effort: [Low/Medium/High]
  - Risk reduction: [percentage or qualitative]

Priority 2 (Short-term):
- [Mitigation 2]
  - Cost: $[amount]
  - Timeline: [timeframe]
  - Effort: [Low/Medium/High]
  - Risk reduction: [percentage or qualitative]

Priority 3 (Long-term):
- [Mitigation 3]
  - Cost: $[amount]
  - Timeline: [timeframe]
  - Effort: [Low/Medium/High]
  - Risk reduction: [percentage or qualitative]

Target Residual Risk (with mitigations): [Low/Medium/High/Critical]

REFERENCES:
- [Industry incidents]
- [CVE numbers if applicable]
- [Threat intelligence reports]
- [Internal documentation]

OWNERSHIP:
Risk Owner: [Name, Title]
Remediation Owner: [Name, Title]
Status: [Open / In Progress / Mitigated / Accepted]
Target Date: YYYY-MM-DD
Review Date: YYYY-MM-DD
```

## D. Risk Register Template

```
Risk ID: R-NNN
Risk Name: [Descriptive name]
Date Identified: YYYY-MM-DD
Identified By: [Name]

ASSET(S):
- [Asset ID]: [Asset Name]

THREAT SCENARIO:
Related Scenario: [TM-YYYY-NNN]
Threat Actor: [Type]
Attack Vector: [Summary]

INHERENT RISK (Before Controls):
Likelihood: [1-5]
Impact: [1-5]
Risk Score: [1-25]
Risk Level: [Low/Medium/High/Critical]

CURRENT CONTROLS:
1. [Control name and description]
   Effectiveness: [Low/Medium/High]

2. [Control name and description]
   Effectiveness: [Low/Medium/High]

Control Effectiveness Overall: [Low/Medium/High]

RESIDUAL RISK (After Current Controls):
Likelihood: [1-5]
Impact: [1-5]
Risk Score: [1-25]
Risk Level: [Low/Medium/High/Critical]

RISK TREATMENT DECISION:
[ ] Mitigate - Implement additional controls
[ ] Accept - Document acceptance rationale
[ ] Transfer - Insurance or outsourcing
[ ] Avoid - Eliminate the risk source

Treatment Rationale:
[Explanation of decision]

PLANNED MITIGATIONS (if Mitigate):
Mitigation 1:
- Description: [What will be implemented]
- Owner: [Name]
- Timeline: [Start - End]
- Cost: $[amount]
- Status: [Not Started / In Progress / Complete / Blocked]
- Expected Risk Reduction: [Likelihood/Impact change]

TARGET RISK (After Planned Mitigations):
Likelihood: [1-5]
Impact: [1-5]
Risk Score: [1-25]
Risk Level: [Low/Medium/High/Critical]

ACCEPTANCE CRITERIA:
Within Risk Appetite: [Yes/No]
Accepted By: [Name, Title] (if accepting residual risk)
Acceptance Date: YYYY-MM-DD

TRACKING:
Status: [Open / In Progress / Closed / Accepted]
Priority: [Critical / High / Medium / Low]
Next Review Date: YYYY-MM-DD
Last Updated: YYYY-MM-DD
Updated By: [Name]

NOTES:
[Any additional context, dependencies, blockers, etc.]
```

## E. Finding Template (Technical)

```
FINDING: F-YYYY-NNN - [Finding Title]

METADATA:
Severity: [Critical / High / Medium / Low / Informational]
CVSS Score: [If applicable]
Category: [Vulnerability type]
Discovery Date: YYYY-MM-DD
Discovered By: [Name/Tool]
Status: [Open / In Progress / Resolved / Accepted]

AFFECTED ASSETS:
- [Asset ID]: [Asset Name]
- [Asset ID]: [Asset Name]

DESCRIPTION:
[Clear description of the vulnerability, misconfiguration, or security gap]

EVIDENCE:
[Commands, screenshots, logs, or other proof]

Example:
$ [command]
[output]

[Screenshot or log snippet]

TECHNICAL DETAILS:
[In-depth technical explanation]
- [Detail 1]
- [Detail 2]
- [Detail 3]

EXPLOITATION:
Exploitability: [Easy / Medium / Hard]
Attack Complexity: [Low / Medium / High]
Privileges Required: [None / Low / High]
User Interaction: [None / Required]

[Description of how this could be exploited]

IMPACT:
Confidentiality: [None / Low / High]
Integrity: [None / Low / High]
Availability: [None / Low / High]

Business Impact:
[Description of business consequences]

Data at Risk:
- [Type and amount of data]

Compliance Impact:
- [Regulations affected]

LIKELIHOOD:
[1-5]: [Very Low / Low / Medium / High / Very High]

Justification:
- [Factor 1]
- [Factor 2]

RISK:
Likelihood × Impact = [Low / Medium / High / Critical]

REMEDIATION:
Immediate Actions (if Critical/High):
1. [Action]
2. [Action]

Permanent Fix:
[Detailed remediation steps]

Step 1:
[Specific technical steps]

Step 2:
[Specific technical steps]

Verification Steps:
1. [How to verify the fix]
2. [Testing procedures]

Alternative Solutions:
- Option 1: [Description, pros/cons]
- Option 2: [Description, pros/cons]

TIMELINE:
Target Resolution: YYYY-MM-DD
Estimated Effort: [Hours/Days/Weeks]
Required Resources: [People, budget, tools]

OWNERSHIP:
Assigned To: [Name, Team]
Business Owner: [Name, Title]
Verifier: [Name]

REFERENCES:
- [CVE numbers]
- [CWE numbers]
- [Documentation]
- [Similar incidents]
- [Best practices]

NOTES:
[Additional context, workarounds, dependencies]
```

## F. Executive Report Template

```
ORGANIZATIONAL THREAT MODEL
Executive Summary Report

[Company Name]
[Report Date]
[Version]

Prepared by: [Consultant/Team Name]
Reviewed by: [Names]

---

EXECUTIVE SUMMARY

Purpose:
[1-2 sentences describing the assessment scope and objectives]

Assessment Period: [Start Date] - [End Date]
Methodology: [STRIDE / PASTA / Combined]

Key Findings:
• [Finding 1 - business impact focused]
• [Finding 2 - business impact focused]
• [Finding 3 - business impact focused]
• [Finding 4 - business impact focused]

Critical Risks:
1. [Risk 1] - $[Impact] potential exposure
2. [Risk 2] - $[Impact] potential exposure
3. [Risk 3] - $[Impact] potential exposure

Overall Security Posture: [WEAK / MODERATE / STRONG]
Trend: [IMPROVING / STABLE / DEGRADING]

---

SCOPE

In Scope:
• [Scope item 1]
• [Scope item 2]
• [Scope item 3]

Out of Scope:
• [Out of scope item 1]
• [Out of scope item 2]

---

ASSETS DISCOVERED

Total Assets: [Number]
• Domains/Subdomains: [Number]
• IP Addresses: [Number]
• Web Applications: [Number]
• APIs: [Number]
• Databases: [Number]
• Repositories: [Number]
• Cloud Resources: [Number]

Asset Criticality:
• Critical: [Number] ([Percentage]%)
• High: [Number] ([Percentage]%)
• Medium: [Number] ([Percentage]%)
• Low: [Number] ([Percentage]%)

---

RISK SUMMARY

[Risk Matrix Visual]

Risk Distribution:
• Critical: [Number] risks ([Percentage]%)
• High: [Number] risks ([Percentage]%)
• Medium: [Number] risks ([Percentage]%)
• Low: [Number] risks ([Percentage]%)

Total Risk Exposure: $[Amount] Annual Loss Expectancy

Top Risks by Impact:
1. [Risk Name] - $[ALE]
2. [Risk Name] - $[ALE]
3. [Risk Name] - $[ALE]
4. [Risk Name] - $[ALE]
5. [Risk Name] - $[ALE]

---

CRITICAL FINDINGS

Finding 1: [Name]
Assets Affected: [Count and type]
Risk Level: CRITICAL

Business Impact:
• [Impact description with dollar amounts]

Recommendation:
• [Action required]
• Timeline: [Immediate / 30 days / 90 days]
• Investment: $[Amount]

---

[Repeat for each critical finding]

---

RECOMMENDATIONS SUMMARY

IMMEDIATE (0-30 days):
Action Items: [Number]
Investment Required: $[Amount]
Risk Reduction: $[Amount ALE reduction]

Priority Actions:
1. [Action] - $[Cost], [Timeline]
2. [Action] - $[Cost], [Timeline]
3. [Action] - $[Cost], [Timeline]

SHORT-TERM (1-3 months):
Action Items: [Number]
Investment Required: $[Amount]
Risk Reduction: $[Amount ALE reduction]

LONG-TERM (3-12 months):
Action Items: [Number]
Investment Required: $[Amount]
Risk Reduction: $[Amount ALE reduction]

---

INVESTMENT AND ROI

Total First-Year Investment: $[Amount]
• Immediate: $[Amount]
• Short-term: $[Amount]
• Long-term: $[Amount]
• Annual recurring: $[Amount]

Risk Reduction:
• Current ALE: $[Amount]
• Post-mitigation ALE: $[Amount]
• Annual risk reduction: $[Amount]

ROI: [Percentage]% or [X]:1
Payback Period: [Timeframe]

---

COMPLIANCE STATUS

PCI-DSS: [Compliant / Gaps Identified / Not Applicable]
• [Number] gaps identified
• [Critical issues if any]

GDPR: [Compliant / Gaps Identified / Not Applicable]
• [Number] gaps identified
• [Critical issues if any]

SOC2: [Compliant / Gaps Identified / Not Applicable]
• [Number] gaps identified
• [Critical issues if any]

HIPAA: [Compliant / Gaps Identified / Not Applicable]
• [Number] gaps identified
• [Critical issues if any]

---

ROADMAP

[Visual timeline or Gantt chart]

Q1 [Year]:
• [Initiative 1]
• [Initiative 2]

Q2 [Year]:
• [Initiative 1]
• [Initiative 2]

Q3-Q4 [Year]:
• [Initiative 1]
• [Initiative 2]

---

NEXT STEPS

1. Review and approve critical findings remediation
2. Allocate budget for priority initiatives
3. Assign ownership for remediation items
4. Schedule monthly progress reviews
5. Plan for quarterly threat model updates

---

APPENDICES

Appendix A: Detailed Risk Register
Appendix B: Asset Inventory
Appendix C: Technical Findings Summary
Appendix D: Methodology Details
```

## G. Tools Reference

### Reconnaissance Tools

```
DOMAIN/SUBDOMAIN ENUMERATION:
- subfinder: github.com/projectdiscovery/subfinder
- amass: github.com/OWASP/Amass
- assetfinder: github.com/tomnomnom/assetfinder
- crt.sh: https://crt.sh (Certificate Transparency)

DNS:
- dig, host, nslookup (built-in)
- fierce: github.com/mschwager/fierce
- dnsrecon: github.com/darkoperator/dnsrecon

HTTP PROBING:
- httpx: github.com/projectdiscovery/httpx
- httprobe: github.com/tomnomnom/httprobe

PORT SCANNING:
- nmap: https://nmap.org
- masscan: github.com/robertdavidgraham/masscan

TECHNOLOGY DETECTION:
- whatweb: github.com/urbanadventurer/WhatWeb
- wappalyzer: https://www.wappalyzer.com
- webanalyze: github.com/rverton/webanalyze

SCREENSHOTS:
- gowitness: github.com/sensepost/gowitness
- aquatone: github.com/michenriksen/aquatone

CLOUD:
- cloud_enum: github.com/initstring/cloud_enum
- S3Scanner: github.com/sa7mon/S3Scanner
- AWS CLI: aws s3 ls --no-sign-request

OSINT:
- theHarvester: github.com/laramies/theHarvester
- spiderfoot: https://www.spiderfoot.net
- recon-ng: github.com/lanmaster53/recon-ng

VULNERABILITY SCANNING:
- nuclei: github.com/projectdiscovery/nuclei
- nikto: https://cirt.net/Nikto2

AUTOMATION:
- Custom bash scripts
- Python automation frameworks
```

### Tool Installation Commands

```bash
# Install Go tools
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
go install -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest

# Install via package managers
apt install nmap whatweb nikto # Debian/Ubuntu
brew install amass nmap # macOS

# Python tools
pip install theHarvester dnsrecon
```

## H. Common Commands Reference

### Subdomain Enumeration

```bash
# subfinder
subfinder -d example.com -o subdomains.txt

# amass passive
amass enum -passive -d example.com -o amass.txt

# Combine and deduplicate
cat subdomains.txt amass.txt | sort -u > all-domains.txt

# Certificate transparency
curl -s "https://crt.sh/?q=%25.example.com&output=json" | \
  jq -r '.[].name_value' | sort -u
```

### HTTP Probing

```bash
# Check which domains are live
httpx -l all-domains.txt -o live.txt

# Get detailed info
httpx -l all-domains.txt -title -tech-detect -status-code -json -o results.json

# Screenshot
gowitness file -f live.txt -P screenshots/
```

### Port Scanning

```bash
# Quick scan common ports
nmap -iL ips.txt -T4 -Pn -p 80,443,22,21,3389,3306,5432 -oA quick-scan

# Full port scan
nmap -iL ips.txt -T4 -Pn -p- -oA full-scan

# Service detection
nmap -iL ips.txt -sV -sC -p 80,443,22 -oA service-scan
```

### S3 Bucket Testing

```bash
# Test for public access
aws s3 ls s3://bucket-name --no-sign-request

# Download bucket
aws s3 sync s3://bucket-name ./local-folder --no-sign-request

# Check bucket permissions
aws s3api get-bucket-acl --bucket bucket-name
```

## I. Engagement Checklist

```
PRE-ENGAGEMENT:
[ ] Contract/SOW signed
[ ] Scope finalized and documented
[ ] Authorization obtained in writing
[ ] Contact list (technical and business)
[ ] Kickoff meeting completed
[ ] Timeline agreed upon
[ ] Deliverable formats confirmed
[ ] Tool access/accounts set up
[ ] Secure storage prepared

DISCOVERY PHASE:
[ ] Reconnaissance checklist completed
[ ] Asset inventory populated
[ ] All findings documented
[ ] Critical findings immediately reported
[ ] Evidence collected and organized
[ ] Discovery methods documented

ANALYSIS PHASE:
[ ] Threat actors identified and profiled
[ ] Attack scenarios documented
[ ] Attack surface mapped
[ ] Threat modeling methodology applied
[ ] Risk assessment completed
[ ] Risk register populated

REPORTING PHASE:
[ ] Executive report drafted
[ ] Technical report drafted
[ ] Presentation created
[ ] Asset inventory finalized
[ ] Risk register finalized
[ ] Internal review completed
[ ] Client draft review
[ ] Feedback incorporated
[ ] Final reports delivered

PRESENTATION:
[ ] Presentation practiced
[ ] Questions anticipated
[ ] Backup materials prepared
[ ] Client questions answered
[ ] Action items documented

POST-DELIVERY:
[ ] Remediation support offered
[ ] Follow-up schedule established
[ ] Lessons learned captured
[ ] Tools and techniques documented
[ ] Archive engagement materials
```

## J. Quick Reference: Risk Matrix

```
5x5 RISK MATRIX

                          IMPACT
              1    2      3       4      5
           Very Low Med   High  V.High
           ┌──────────────────────────────┐
         5 │  M  │  H  │   H   │  C  │  C │ Very High
LIKELIHOOD├──────┼─────┼───────┼─────┼────┤
         4 │  M  │  M  │   H   │  H  │  C │ High
           ├──────┼─────┼───────┼─────┼────┤
         3 │  L  │  M  │   M   │  H  │  H │ Medium
           ├──────┼─────┼───────┼─────┼────┤
         2 │  L  │  L  │   M   │  M  │  H │ Low
           ├──────┼─────┼───────┼─────┼────┤
         1 │  L  │  L  │   L   │  L  │  M │ Very Low
           └──────────────────────────────┘

L = Low Risk (Score: 1-4)
M = Medium Risk (Score: 5-9)
H = High Risk (Score: 10-16)
C = Critical Risk (Score: 17-25)

LIKELIHOOD SCALE:
5 - Very High: Almost certain, constant occurrence
4 - High: Likely to occur, common in industry
3 - Medium: Possible, occasional occurrence
2 - Low: Unlikely, rare occurrence
1 - Very Low: Extremely rare, no precedent

IMPACT SCALE:
5 - Very High: >$10M, catastrophic business impact
4 - High: $1M-$10M, significant impact
3 - Medium: $100k-$1M, moderate impact
2 - Low: $10k-$100k, minor impact
1 - Very Low: <$10k, negligible impact
```

## K. Compliance Mapping Quick Reference

```
PCI-DSS REQUIREMENTS:
Requirement 1: Firewalls
Requirement 2: Default credentials
Requirement 3: Stored cardholder data
Requirement 4: Encryption in transit
Requirement 5: Antimalware
Requirement 6: Secure development
Requirement 7: Access controls
Requirement 8: Authentication
Requirement 9: Physical access
Requirement 10: Logging
Requirement 11: Testing
Requirement 12: Security policy

GDPR KEY ARTICLES:
Article 5: Data processing principles
Article 25: Privacy by design
Article 30: Records of processing
Article 32: Security of processing
Article 33: Breach notification (72 hours)
Article 34: Communication to data subjects

SOC2 TRUST PRINCIPLES:
Security: Protected against unauthorized access
Availability: System available for operation and use
Processing Integrity: System processing is complete, valid, accurate, timely, authorized
Confidentiality: Confidential information is protected
Privacy: Personal information is collected, used, retained, disclosed, and disposed properly

HIPAA SECURITY RULE:
Administrative Safeguards: Risk analysis, workforce training, incident response
Physical Safeguards: Facility access, workstation security, device controls
Technical Safeguards: Access control, audit controls, integrity, transmission security
```

---

End of Templates and Checklists