# Chapter 6: Threat Modeling Methodologies

## Overview

You have assets, threats, and attack surface mapped. Now apply formal methodologies to systematically model threats.

Multiple frameworks exist for threat modeling. Each has strengths and weaknesses. This chapter covers the most common methodologies and when to use them. Pick the one that fits your engagement, or combine multiple approaches.

## STRIDE

STRIDE is Microsoft's threat modeling framework. It categorizes threats into six types.

### STRIDE Categories

**S - Spoofing:**
Impersonating something or someone.

Examples:
- Spoofing email addresses
- Fake login pages (phishing)
- IP address spoofing
- Session token theft and replay
- Impersonating users or systems

Questions to ask:
- How does the system authenticate users?
- Can authentication be bypassed?
- Can one user impersonate another?
- Can external systems be spoofed?

**T - Tampering:**
Modifying data or code.

Examples:
- SQL injection modifying database
- Man-in-the-middle attacks
- Modifying application code
- Changing configuration files
- Altering log files
- Parameter manipulation

Questions to ask:
- What data can be modified?
- Is data integrity verified?
- Can data be changed in transit?
- Can users modify other users' data?

**R - Repudiation:**
Denying actions or transactions.

Examples:
- User denies making a purchase
- Admin denies configuration changes
- Attacker clears logs of actions
- Unsigned transactions

Questions to ask:
- Are actions logged?
- Can logs be altered or deleted?
- Are logs tamper-proof?
- Can users deny legitimate actions?

**I - Information Disclosure:**
Exposing information to unauthorized users.

Examples:
- Exposed database backup
- Error messages revealing stack traces
- Unencrypted data transmission
- Data in URL parameters
- Public S3 buckets

Questions to ask:
- What sensitive data exists?
- Who should access it?
- Is it encrypted in transit and at rest?
- Can unauthorized users access it?

**D - Denial of Service:**
Disrupting system availability.

Examples:
- DDoS attacks
- Resource exhaustion
- Application crashes
- Database overload
- Bandwidth saturation

Questions to ask:
- What happens under heavy load?
- Are there rate limits?
- Can the system be crashed?
- What's the recovery process?

**E - Elevation of Privilege:**
Gaining unauthorized permissions.

Examples:
- SQL injection to gain admin access
- Privilege escalation exploits
- Exploiting sudo misconfigurations
- JWT token manipulation
- IDOR to access admin functions

Questions to ask:
- How are privileges assigned?
- Can users elevate their privileges?
- Are admin functions properly protected?
- Can low-privilege users access high-privilege data?

### Applying STRIDE

**Per-asset analysis:**
```
Asset: Customer API (api.example.com)

Spoofing:
- Threat: Attacker steals API key and impersonates legitimate client
- Attack: API key extracted from mobile app, used by attacker
- Impact: Unauthorized API access, potential data theft
- Mitigation: Rotate keys, implement additional auth (device fingerprinting)

Tampering:
- Threat: Attacker modifies API requests to change data
- Attack: Intercept and modify order amount in POST /orders
- Impact: Financial loss, incorrect transactions
- Mitigation: Request signing, HTTPS enforcement, input validation

Repudiation:
- Threat: User denies making API call
- Attack: User claims they didn't initiate transaction
- Impact: Dispute resolution difficulty, potential fraud
- Mitigation: Detailed audit logging, request signatures, timestamps

Information Disclosure:
- Threat: API leaks sensitive customer data
- Attack: IDOR in GET /users/{id} returns any user's data
- Impact: Customer PII exposure, privacy violation
- Mitigation: Authorization checks, data filtering, access controls

Denial of Service:
- Threat: Attacker overwhelms API with requests
- Attack: Flood API endpoints with automated requests
- Impact: Service degradation, revenue loss
- Mitigation: Rate limiting, DDoS protection, caching

Elevation of Privilege:
- Threat: Regular user accesses admin API endpoints
- Attack: Call POST /admin/users without proper auth check
- Impact: Unauthorized admin actions, system compromise
- Mitigation: Strict authorization checks, role-based access control
```

**Data flow analysis:**
Apply STRIDE to each data flow.

```
Data Flow: User Login -> Auth Service -> User Database

Spoofing:
- User could be fake (phishing harvested creds)
- Auth service could be spoofed (DNS poisoning)
- Database connection could be MitM'd

Tampering:
- Login credentials intercepted and modified
- Database queries modified (SQL injection)
- Response data altered

Repudiation:
- Login attempts not logged
- Cannot prove who authenticated

Information Disclosure:
- Credentials transmitted in cleartext
- Database query reveals user data in error
- Session token exposed

Denial of Service:
- Login endpoint flooded
- Auth service overwhelmed
- Database connection pool exhausted

Elevation of Privilege:
- SQL injection grants admin access
- Session token manipulation elevates role
```

## PASTA

Process for Attack Simulation and Threat Analysis. Risk-centric methodology focused on business objectives.

### PASTA Seven Stages

**Stage 1: Define Objectives**

Identify business objectives and security requirements.

```
Business Objectives:
- Maintain customer trust
- Process payments securely
- Protect customer PII
- Ensure 99.9% uptime
- Comply with PCI-DSS and GDPR

Security Objectives:
- Prevent data breaches
- Maintain service availability
- Protect payment card data
- Ensure user privacy
- Detect and respond to incidents within 1 hour

Success Criteria:
- No data breaches
- No PCI compliance violations
- No GDPR violations
- SLA maintained
- All security controls operational
```

**Stage 2: Define Technical Scope**

Map technical boundaries and components.

```
Technical Scope:

In Scope:
- Web application (app.example.com)
- API services (api.example.com)
- Customer database (PostgreSQL RDS)
- Payment processing integration (Stripe)
- AWS infrastructure (us-east-1)

Out of Scope:
- Stripe's internal systems
- AWS infrastructure security (shared responsibility)
- Physical security of data centers
- Employee workstations (separate assessment)

Architecture Components:
- Load balancer (AWS ALB)
- Web servers (EC2 instances)
- Application servers (ECS containers)
- Database (RDS PostgreSQL)
- Cache (ElastiCache Redis)
- Storage (S3)
- CDN (CloudFront)

Trust Boundaries:
- Internet <-> CDN
- CDN <-> Load Balancer
- Load Balancer <-> Application
- Application <-> Database
- Application <-> Third-party APIs
```

**Stage 3: Application Decomposition**

Break down the application into components.

```
Application: E-Commerce Platform

Components:
1. Frontend (React SPA)
   - User interface
   - Client-side validation
   - API communication

2. API Gateway
   - Request routing
   - Authentication
   - Rate limiting

3. User Service
   - User management
   - Authentication
   - Profile management

4. Product Service
   - Product catalog
   - Search
   - Inventory

5. Order Service
   - Shopping cart
   - Order processing
   - Order history

6. Payment Service
   - Payment processing (Stripe integration)
   - PCI scope boundary
   - Transaction records

7. Database Layer
   - PostgreSQL (users, products, orders)
   - Redis (sessions, cache)

8. External Integrations
   - Stripe (payments)
   - SendGrid (email)
   - AWS S3 (file storage)
```

**Stage 4: Threat Analysis**

Identify threats using threat intelligence and attack libraries.

```
Threat Intelligence:
- Recent e-commerce platform breaches (Magecart, etc.)
- PCI DSS requirements and common violations
- OWASP Top 10 for web applications
- Industry threat reports

Applicable Threats:
1. Payment card data theft (Magecart-style attacks)
2. Customer data breaches (IDOR, SQLi)
3. Account takeover (credential stuffing)
4. Business logic flaws (price manipulation)
5. Supply chain attacks (compromised dependencies)
6. API abuse (scraping, DoS)
7. Insider threats (data exfiltration)

For each threat:
- Threat actor
- Motivation
- Capabilities required
- Historical precedent
```

**Stage 5: Vulnerability Analysis**

Identify weaknesses that enable threats.

```
Vulnerability Assessment:

Application Vulnerabilities:
- No rate limiting on login (enables credential stuffing)
- IDOR in order retrieval API (data exposure)
- Weak password policy (account compromise)
- No MFA option (account takeover risk)

Infrastructure Vulnerabilities:
- Outdated packages with known CVEs
- S3 buckets with public read (misconfiguration)
- Overly permissive IAM roles
- No database query parameterization in legacy code

Process Vulnerabilities:
- Manual deployment process (human error risk)
- No code review requirement (malicious code risk)
- Insufficient access logging (detection gap)
- No incident response plan (slow response)

For each vulnerability:
- CVSS score (if applicable)
- Exploitability
- Impact
- Affected components
```

**Stage 6: Attack Modeling**

Simulate attacks using attack trees and scenarios.

```
Attack Scenario: Payment Card Data Theft

Attack Tree:
Goal: Steal Payment Card Data

OR:
├─ Path 1: Compromise Payment Service
│  └─ AND:
│     ├─ Exploit SQL injection in order lookup
│     ├─ Access payment database
│     └─ Exfiltrate card data
│
├─ Path 2: JavaScript Injection (Magecart)
│  └─ AND:
│     ├─ Compromise third-party script
│     ├─ Inject card skimming code
│     └─ Exfiltrate to attacker domain
│
└─ Path 3: API Abuse
   └─ AND:
      ├─ Obtain valid API key
      ├─ Call payment API directly
      └─ Extract card tokens

For each path:
- Likelihood: High/Medium/Low
- Difficulty: Easy/Medium/Hard
- Detection probability: High/Medium/Low
- Attacker profile: Script kiddie/Criminal/APT
```

**Stage 7: Risk and Impact Analysis**

Calculate risk and prioritize.

```
Risk Formula: Risk = Likelihood × Impact

Threat: Payment Card Data Theft (Path 2: Magecart)

Likelihood: Medium
- Third-party scripts in use
- No SRI implementation
- Historical precedent

Impact: Critical
- PCI compliance violation
- Customer data breach
- Financial liability (fines, lawsuits)
- Reputation damage
- Business disruption

Risk Score: HIGH

Residual Risk (with controls):
- Implement SRI
- CSP to block unauthorized scripts
- Regular third-party script audits

Residual Likelihood: Low
Residual Impact: Critical
Residual Risk: MEDIUM

Risk Acceptance: No - implement controls
```

## Attack Trees

Visual representation of attack paths.

### Building Attack Trees

```
Goal: Compromise Production Database

                    [Root: Compromise Database]
                              |
        ┌─────────────────────┴─────────────────────┐
        │                                             │
    [Via Application]                          [Via Infrastructure]
        │                                             │
    ┌───┴───┐                                     ┌──┴──┐
    │       │                                     │     │
[SQLi]  [Backup]                              [SSH]  [Cloud]
            │                                     │      │
        ┌───┴───┐                             ┌───┴───┐  │
        │       │                             │       │  │
    [S3]    [Export]                      [Brute] [Stolen] │
                                                         [IAM]

Leaf Nodes (Attack Vectors):
1. SQL Injection in application
2. Access S3 backup bucket (misconfigured)
3. Database export feature (IDOR)
4. SSH brute force (weak password)
5. Stolen SSH key (from repo)
6. Compromise AWS IAM credentials

AND/OR Logic:
- OR gates: Any path succeeds
- AND gates: All conditions required

Each leaf has:
- Likelihood
- Difficulty
- Detection probability
- Mitigation status
```

### Attack Tree Analysis

```
Path Analysis:

Easiest Path:
S3 Backup Access
- Difficulty: Easy (just enumerate buckets)
- Likelihood: High (common misconfiguration)
- Detection: Low (no alerting on bucket access)
- Mitigation: Fix bucket permissions (IMMEDIATE)

Most Likely Path:
SQL Injection
- Difficulty: Medium (need to find injection point)
- Likelihood: Medium (depending on code quality)
- Detection: Medium (WAF might catch)
- Mitigation: Code review, parameterized queries, WAF

Hardest Path:
SSH Brute Force
- Difficulty: Hard (strong passwords, possible fail2ban)
- Likelihood: Low (time-consuming, noisy)
- Detection: High (obvious in logs)
- Mitigation: Already mitigated if strong passwords + monitoring
```

## Kill Chain Analysis

Map attacks to cyber kill chain phases.

### Lockheed Martin Kill Chain

```
Phase 1: Reconnaissance
Actions:
- Subdomain enumeration
- Port scanning
- Technology fingerprinting
- Employee identification

Detections:
- Scan detection (frequency-based)
- Honeypots for enumeration attempts

Disruption:
- Rate limiting
- Geo-blocking
- Deception technology

Phase 2: Weaponization
Actions:
- Prepare exploit payload
- Create phishing email
- Register lookalike domains

Detections:
- Threat intelligence (IOC matching)
- Domain monitoring

Disruption:
- Difficult (happens off-network)

Phase 3: Delivery
Actions:
- Send phishing email
- Exploit exposed service
- Watering hole attack

Detections:
- Email filtering
- Link analysis
- Attachment sandboxing

Disruption:
- Email security gateway
- User awareness training
- Endpoint protection

Phase 4: Exploitation
Actions:
- Execute malicious code
- Exploit vulnerability
- Social engineering success

Detections:
- Endpoint detection (EDR)
- Behavior analysis
- Exploit prevention

Disruption:
- Patch management
- Application whitelisting
- Endpoint hardening

Phase 5: Installation
Actions:
- Install malware
- Create persistence
- Establish backdoor

Detections:
- File integrity monitoring
- Registry monitoring
- Startup item analysis

Disruption:
- Application control
- Privilege restrictions
- Host-based firewalls

Phase 6: Command & Control
Actions:
- Beacon to C2 server
- Receive commands
- Exfiltrate data

Detections:
- Network traffic analysis
- DNS monitoring
- Proxy logs

Disruption:
- Firewall rules
- DNS filtering
- Proxy controls

Phase 7: Actions on Objectives
Actions:
- Data exfiltration
- Lateral movement
- Destructive actions

Detections:
- Data loss prevention
- Unusual data transfers
- Privileged account monitoring

Disruption:
- Network segmentation
- Data encryption
- Access controls
```

### Disruption Opportunities

For each phase, identify where you can disrupt the attack.

Early phases (Reconnaissance, Weaponization, Delivery):
- Less visibility
- Harder to detect
- Lower confidence in detection

Later phases (Installation, C2, Actions):
- Better visibility
- Higher confidence
- But attacker is already in

Goal: Detect and disrupt as early as possible.

## MITRE ATT&CK Mapping

Map observed or potential attacks to ATT&CK framework.

### ATT&CK Navigator

```
Tactics (High Level):
1. Initial Access
2. Execution
3. Persistence
4. Privilege Escalation
5. Defense Evasion
6. Credential Access
7. Discovery
8. Lateral Movement
9. Collection
10. Command and Control
11. Exfiltration
12. Impact

For the organization, map applicable techniques:

Initial Access:
- T1190: Exploit Public-Facing Application (web app vulns)
- T1566: Phishing (email-based attacks)
- T1078: Valid Accounts (credential stuffing)
- T1133: External Remote Services (VPN compromise)

Execution:
- T1059: Command and Scripting Interpreter (if compromised)
- T1203: Exploitation for Client Execution (browser exploits)

Persistence:
- T1136: Create Account (create backdoor user)
- T1078: Valid Accounts (compromised credentials)
- T1505: Server Software Component (web shell)

[Continue for each tactic...]
```

### Technique Prioritization

```
For Each Technique:

T1190: Exploit Public-Facing Application

Relevance: HIGH
- Multiple public-facing web apps and APIs
- Known vulnerabilities in dependencies
- Primary attack vector

Detection Coverage:
- WAF deployed (partial coverage)
- No anomaly detection
- Application logging (basic)
Assessment: GAPS EXIST

Mitigation Status:
- Patch management (irregular)
- Input validation (varies by component)
- WAF rules (basic)
Assessment: NEEDS IMPROVEMENT

Priority: HIGH
Actions:
1. Implement automated vulnerability scanning
2. Enhance WAF rules
3. Improve patch management process
4. Deploy runtime application protection
```

## OCTAVE

Operationally Critical Threat, Asset, and Vulnerability Evaluation. Risk-based approach focused on organizational risk.

### OCTAVE Process

**Phase 1: Build Asset-Based Threat Profiles**

```
Critical Asset: Customer Database

Asset Description:
- PostgreSQL database
- Contains customer PII, order history, payment tokens
- RDS instance in AWS us-east-1
- Accessed by application servers only (intended)

Asset Owner: Engineering - Data Team

Security Requirements:
- Confidentiality: HIGH (customer PII)
- Integrity: HIGH (transaction data)
- Availability: MEDIUM (can tolerate brief outages)

Threats:
- Data breach (confidentiality violation)
- Data corruption (integrity violation)
- Database unavailability (availability violation)
- Insider data theft (confidentiality violation)

Current Practices:
- Encryption at rest (AWS KMS)
- Encryption in transit (SSL)
- Backups (daily, 30-day retention)
- Access controls (limited to app service accounts)
- Audit logging (RDS logs to CloudWatch)

Gaps:
- No database activity monitoring
- No data access anomaly detection
- Backup encryption not verified
- No regular access review
```

**Phase 2: Identify Infrastructure Vulnerabilities**

```
Infrastructure Review:

Network:
- Flat network (weak segmentation)
- Database accessible from application tier (correct)
- VPN provides access to internal network (overly broad)

Systems:
- RDS: PostgreSQL 13.7 (current, good)
- Application servers: Various versions (inconsistent)
- No EDR on all systems

Applications:
- Web app: Known OWASP Top 10 vulns
- API: No rate limiting
- Admin panel: No MFA requirement

Vulnerabilities Found:
- CVE-2023-XXXX: RDS minor version behind (LOW)
- Weak network segmentation (HIGH)
- Application vulnerabilities (MEDIUM-HIGH)
- Insufficient access controls on VPN (MEDIUM)
```

**Phase 3: Develop Security Strategy and Plans**

```
Risk Evaluation:

Asset: Customer Database
Threat: Data Breach via SQL Injection

Impact: CRITICAL
- Customer data exposed
- PCI and GDPR violations
- Reputation damage
- Financial loss (fines, lawsuits)

Probability: MEDIUM
- Some input validation exists
- WAF provides some protection
- Code review not consistent
- Historical SQLi in industry

Risk: HIGH

Mitigation Strategy:
- Immediate: Code review for SQLi vulnerabilities
- Short-term: Implement parameterized queries everywhere
- Medium-term: Database activity monitoring
- Long-term: Zero trust network architecture

Risk Owner: CTO
Implementation Owner: VP Engineering
Timeline: Q1 2026
Budget: $50k
```

## Choosing a Methodology

### Selection Criteria

**Use STRIDE when:**
- Need comprehensive threat coverage
- Analyzing specific applications or systems
- Working with Microsoft-centric environments
- Team familiar with STRIDE
- Want structured categorization

**Use PASTA when:**
- Risk-focused assessment required
- Need to align security with business objectives
- Want to include business stakeholders
- Comprehensive seven-stage analysis fits timeline
- Compliance-driven requirements

**Use Attack Trees when:**
- Visual representation needed
- Analyzing specific attack scenarios
- Communicating with non-technical stakeholders
- Need to identify easiest attack paths
- Prioritizing defenses based on attack difficulty

**Use Kill Chain when:**
- Want to identify detection and disruption points
- Planning defensive layers
- Incident response planning
- Understanding attack progression
- Deploying defensive technologies

**Use MITRE ATT&CK when:**
- Mapping real-world attack techniques
- Gap analysis for detection and prevention
- Integrating with security operations
- Threat hunting
- Comparing against known threat actor TTPs

**Use OCTAVE when:**
- Organizational risk focus
- Business-driven security requirements
- Need to involve business stakeholders
- Operational security emphasis
- Resource allocation decisions

### Combining Methodologies

Often best to combine approaches:

```
Hybrid Approach:

1. PASTA (Stages 1-3): Define objectives and scope
2. STRIDE: Identify threats per component
3. MITRE ATT&CK: Map techniques and detection gaps
4. Attack Trees: Model high-priority attack paths
5. Kill Chain: Identify disruption points
6. Risk Assessment: Prioritize (Chapter 7)

Benefits:
- Comprehensive coverage
- Multiple perspectives
- Better communication (different audiences)
- Thorough analysis
```

## Practical Application

### Threat Modeling Workshop

**Participants:**
- Security team (lead the session)
- Engineering team (understand the systems)
- Product team (understand business context)
- Operations team (understand infrastructure)

**Agenda:**
```
Hour 1: Context Setting
- Review assets and architecture
- Explain threat modeling methodology
- Define scope for session

Hour 2-3: Threat Identification
- Work through methodology (STRIDE, etc.)
- Document threats per component
- Identify attack paths

Hour 4: Prioritization and Planning
- Assess likelihood and impact
- Prioritize threats
- Identify mitigation strategies
- Assign ownership

Output:
- Threat model document
- Risk register
- Action items with owners
```

### Documentation Templates

**Threat Scenario Template:**
```
Threat ID: TM-001
Threat Name: Credential Stuffing Attack on Customer Portal

Asset: Customer login portal (app.example.com/login)

Threat Actor: Financially motivated criminals

Attack Vector:
- Obtain breached credentials from dark web
- Use automated tools (Sentry MBA, SNIPR, etc.)
- Test credentials against login portal
- Identify valid accounts
- Access customer data or conduct fraud

Prerequisites:
- No rate limiting on login endpoint
- No CAPTCHA requirement
- No account lockout after failed attempts
- Weak password policy (users reuse passwords)

Attack Steps:
1. Acquire credential lists (1M+ username/password pairs)
2. Set up automated credential stuffing tool
3. Configure tool to target app.example.com/login
4. Test credentials at moderate rate (avoid detection)
5. Harvest valid credentials
6. Access accounts and exfiltrate data or commit fraud

Indicators:
- High volume of failed login attempts from diverse IPs
- Successful logins from unusual geographic locations
- Multiple accounts accessed from same IP
- User agents indicative of automation

Impact:
- Account takeover
- Customer data exposure
- Fraudulent transactions
- Reputation damage
- Support costs (password resets, investigations)

Likelihood: HIGH
- Common attack type
- Easy to execute
- Low technical barrier
- Tools readily available

Impact: HIGH
- Customer data at risk
- Financial loss
- Compliance implications

Risk: CRITICAL

Existing Controls:
- HTTPS (prevents credential interception)
- Password complexity (basic)

Control Gaps:
- No rate limiting
- No CAPTCHA
- No account lockout
- No anomaly detection
- MFA not enforced

Recommended Mitigations:
1. Implement rate limiting (per IP and per account)
2. Add CAPTCHA after N failed attempts
3. Implement account lockout (temporary)
4. Deploy anomaly detection (impossible travel, etc.)
5. Enforce MFA for all accounts
6. Password strength requirements (pwned password check)
7. Monitor for credential stuffing campaigns

Priority: HIGH
Owner: Security Team
Due Date: Q1 2026

STRIDE Mapping: Spoofing
ATT&CK Technique: T1078.001 - Valid Accounts: Default Accounts
Kill Chain Phase: Initial Access
```

## Outputs

### Threat Model Document

Comprehensive document including:
- Executive summary
- Methodology used
- Assets analyzed
- Threats identified (categorized by framework)
- Attack scenarios
- Risk ratings
- Recommendations

### Threat Register

Spreadsheet tracking all identified threats:
```
Threat ID | Threat Name | Asset | Category | Likelihood | Impact | Risk | Status | Owner
```

### Visual Diagrams

- Architecture diagrams with trust boundaries
- Attack trees for critical scenarios
- Data flow diagrams with threat annotations
- Kill chain mapping

## Next Steps

Threat modeling complete. Every threat documented and categorized using formal methodologies.

Chapter 7 covers risk assessment and prioritization - calculating risk scores, prioritizing threats, and determining which risks require immediate action versus acceptance.

You have the threats. Now calculate the risk.