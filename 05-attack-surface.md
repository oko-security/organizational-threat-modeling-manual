# Chapter 5: Attack Surface Analysis

## Overview

The attack surface is every point where an attacker could interact with your systems. Every login page, every API endpoint, every employee email address, every exposed port - they're all entry points.

This chapter maps the attack surface across external systems, internal systems, people, and supply chain. The goal is to understand exactly how threat actors from Chapter 4 could reach the assets from Chapter 3.

## Attack Surface Categories

### External Attack Surface

Anything accessible from the internet without prior authentication or access.

**Web applications:**
- Public websites
- Customer portals
- Marketing sites
- Documentation sites
- Blog platforms

**APIs:**
- REST endpoints
- GraphQL interfaces
- SOAP services
- WebSocket connections
- Webhook receivers

**Mail infrastructure:**
- MX servers
- Webmail interfaces
- SMTP endpoints
- Mail API gateways

**Remote access:**
- VPN gateways
- Remote desktop gateways
- SSH jump hosts
- Bastion servers

**DNS:**
- Authoritative nameservers
- DNS records revealing infrastructure
- Zone transfer vulnerabilities

**Other exposed services:**
- FTP servers
- File sharing endpoints
- Database ports (if exposed)
- Message queues
- Monitoring interfaces
- Management consoles

### Internal Attack Surface

Systems accessible only after initial access is gained.

**Internal applications:**
- HR systems
- Finance applications
- Internal tools and dashboards
- Development environments

**Internal infrastructure:**
- Application servers
- Database servers
- File servers
- Directory services (Active Directory)
- Internal APIs

**Management interfaces:**
- Server management (iLO, iDRAC, IPMI)
- Network equipment (switches, routers)
- Security appliances
- Hypervisor management

### Human Attack Surface

People are the softest target.

**Email addresses:**
- Employee corporate emails
- Distribution lists
- Support addresses
- Executive emails

**Social media accounts:**
- Corporate accounts
- Employee personal accounts
- LinkedIn profiles
- Twitter/X accounts

**Phone numbers:**
- Corporate numbers
- Employee mobile numbers
- Support lines

**Physical presence:**
- Office locations
- Employee movements
- Tailgating opportunities
- Dumpster diving

### Supply Chain Attack Surface

Third-party relationships create indirect exposure.

**Vendor access:**
- Support portal access
- VPN connections
- Shared systems
- API integrations

**Software dependencies:**
- Open source packages
- Commercial libraries
- Third-party SDKs
- Container images

**SaaS platforms:**
- Cloud infrastructure providers
- Development tools
- Productivity suites
- Communication platforms

**Service providers:**
- MSPs and consultants
- Contractors
- Temporary staff
- Business partners

## External Attack Surface Mapping

Document every external entry point.

### Web Application Entry Points

For each web application:

**Authentication mechanisms:**
```
Application: app.example.com
Login URL: https://app.example.com/login

Authentication Methods:
- Username/password
- OAuth (Google, Microsoft)
- SAML SSO (for enterprise customers)
- API key authentication

Password Requirements:
- Minimum 8 characters (weak)
- No complexity requirements observed
- No account lockout observed
- No CAPTCHA on login

MFA Status: Optional (not enforced)

Session Management:
- Cookie-based sessions
- Session timeout: 7 days (long)
- No HttpOnly flag on session cookie (vulnerability)

Attack Vectors:
- Credential stuffing (no rate limiting)
- Phishing (MFA not required)
- Session hijacking (insecure cookie)
- Brute force (no lockout)
```

**Input points:**
Every form field and API parameter is an input point.

```
Registration Form (/register):
- Email address (XSS potential)
- Password (tested: accepts weak passwords)
- First name, Last name (tested: accepts special chars)
- Company name
- Phone number

Profile Update (/api/profile):
- Profile photo upload (file upload vulnerability risk)
- Bio field (tested: accepts HTML, XSS risk)
- Website URL
- Social media links

Search Functionality (/search):
- Query parameter (SQL injection risk)
- Filters (category, date, etc.)
- Sorting parameters

Payment Form (/checkout):
- Card number
- CVV
- Billing address
- (Note: Uses Stripe.js, direct PCI exposure limited)
```

**Unauthenticated areas:**
Parts of the app accessible without logging in.

```
Public Endpoints:
- / (homepage)
- /about
- /pricing
- /blog/*
- /api/v1/public/*
- /health (status check - reveals app version)
- /.well-known/* (ACME challenges, etc.)

Information Disclosure:
- /api/v1/health shows database connection status
- /api/docs exposes API documentation (should be auth-required)
- /debug (403 but exists, potential target)
```

### API Attack Surface

**Endpoint enumeration:**
```
API Base: https://api.example.com/v1

Discovered Endpoints:
- POST /auth/login (authentication)
- POST /auth/register (account creation)
- GET /users/{id} (user profile)
- PUT /users/{id} (profile update)
- GET /products (product listing)
- POST /orders (order creation)
- GET /orders/{id} (order retrieval)
- POST /payments (payment processing)
- GET /admin/users (admin function, auth check?)

Enumeration Method:
- API documentation (/api/docs)
- JavaScript file analysis
- Fuzzing common patterns
- OpenAPI/Swagger spec

Authentication:
- Bearer token in Authorization header
- API keys in X-API-Key header (for M2M)
- No authentication (for public endpoints)

Rate Limiting:
- Observed: 1000 requests/hour per IP
- Bypassable via rotating IPs?
- No user-based rate limiting observed

CORS Policy:
- Allows all origins (Access-Control-Allow-Origin: *)
- Potential for cross-site attacks

Versioning:
- /v1/ currently in use
- /v2/ exists (beta, undocumented)
- Legacy /api/ still responds (deprecated, potentially vulnerable)
```

**API vulnerabilities:**
Common API-specific attack vectors.

```
IDOR (Insecure Direct Object Reference):
- GET /users/{id} returns any user with valid ID
- GET /orders/{id} no ownership check observed
- Potential for data enumeration

Mass Assignment:
- POST /users creates user
- Test: can role be set via API? (privilege escalation)

Excessive Data Exposure:
- GET /users/{id} returns full user object
- Includes email, phone, internal IDs
- Should filter sensitive fields

GraphQL (if applicable):
- Introspection enabled (schema disclosure)
- No depth limiting (nested query DoS)
- No complexity limits

Parameter Pollution:
- Test duplicate parameters
- Array handling in queries
```

### Exposed Services and Ports

**Port scan results:**
```
Host: app.example.com (54.210.123.45)

Open Ports:
22/tcp   SSH      OpenSSH 8.2p1
80/tcp   HTTP     nginx 1.18.0 (redirects to 443)
443/tcp  HTTPS    nginx 1.18.0

Host: mail.example.com (203.0.113.50)

Open Ports:
25/tcp   SMTP     Postfix
143/tcp  IMAP     Dovecot 2.3.13
993/tcp  IMAPS    Dovecot 2.3.13
587/tcp  SMTP     Postfix (submission)

Host: legacy.example.com (198.51.100.75)

Open Ports:
22/tcp   SSH      OpenSSH 7.4 (outdated)
80/tcp   HTTP     Apache 2.4.6 (outdated)
443/tcp  HTTPS    Apache 2.4.6
3306/tcp MySQL    (EXPOSED TO INTERNET - CRITICAL)

Risk Assessment:
- MySQL exposed on legacy.example.com is CRITICAL
- Outdated SSH and Apache on legacy system
- Otherwise minimal exposed services (good)
```

**Service-specific attack vectors:**

SSH:
- Password authentication enabled? (test)
- Key-based authentication? (preferred)
- Version vulnerable to known exploits?
- User enumeration possible?

HTTP/HTTPS:
- Server version disclosure
- Known vulnerabilities in web server
- SSL/TLS configuration weaknesses
- Cipher suite issues

Email:
- Open relay testing
- User enumeration via VRFY/EXPN
- Bounce message information disclosure
- SPF/DKIM/DMARC bypass attempts

### DNS Attack Surface

**DNS records as attack surface:**
```
example.com DNS Records:

A Records:
- example.com -> 104.21.12.34 (Cloudflare)
- www.example.com -> 104.21.12.34 (Cloudflare)
- mail.example.com -> 203.0.113.50 (direct IP, origin exposed)

CNAME Records:
- app.example.com -> lb-prod.us-east-1.elb.amazonaws.com
  (Reveals AWS usage, region, naming convention)
- api.example.com -> api-prod-lb-12345.us-east-1.elb.amazonaws.com
- blog.example.com -> example-com.ghost.io (Ghost hosting)

MX Records:
- 10 aspmx.l.google.com (Google Workspace)
- Reveals email provider, potential phishing target

TXT Records:
- SPF: "v=spf1 include:_spf.google.com include:sendgrid.net ~all"
  (Reveals SendGrid usage for transactional email)
- DMARC: "v=DMARC1; p=none; rua=mailto:dmarc@example.com"
  (Policy is 'none' - not enforcing)

SRV Records:
- _sip._tcp.example.com (VoIP service in use)

Attack Vectors:
- Subdomain enumeration (cert transparency, brute force)
- Zone transfer attempts
- DNS cache poisoning
- Subdomain takeover (check CNAMEs for unclaimed resources)
```

**Subdomain takeover risks:**
```
Checking: app-old.example.com

CNAME: app-old-12345.herokuapp.com
Result: "No such app" (VULNERABLE)

Risk: Can claim heroku app name, serve malicious content
Priority: HIGH - immediate remediation needed

Other Potential Takeovers:
- staging-v2.example.com -> CNAME to deleted S3 bucket
- docs-old.example.com -> GitHub Pages (repo deleted)
```

### Cloud Infrastructure Exposure

**S3 buckets:**
```
Discovered Buckets:
- example-com-assets (public read - INTENDED)
  Contents: Images, CSS, JS
  Risk: Low (intended public access)

- example-com-backups (public read - UNINTENDED)
  Contents: Database dumps (2023-2024)
  Risk: CRITICAL - immediate remediation
  Finding: Contains customer data, PII, credentials

- example-com-logs (listable - MISCONFIGURED)
  Contents: Application logs
  Risk: HIGH - logs contain session tokens, API keys

Bucket Permission Audit:
✓ example-com-assets: Public read appropriate
✗ example-com-backups: CRITICAL - should be private
✗ example-com-logs: HIGH - should be private
✓ example-com-uploads: Private (correct)
```

**Azure blob storage:**
```
Account: examplecomstorage.blob.core.windows.net

Public Containers:
- static (public) - INTENDED
- backups (listable) - MISCONFIGURED

Similar findings to S3 analysis.
```

**Google Cloud Storage:**
```
Bucket: example-com-prod-data

Access: allUsers (MISCONFIGURED)
Finding: Production database exports accessible
```

**Cloud service enumeration:**
```
AWS Resources (via subdomain analysis):
- us-east-1 (primary region)
- eu-west-1 (secondary region)
- Resources: ELB, RDS, ElastiCache, S3, CloudFront

Azure Resources:
- East US region
- Resources: App Services, SQL Database

GCP Resources:
- us-central1
- Resources: Cloud Storage, Cloud Functions

Multi-cloud finding: Complex attack surface across providers
```

### Mobile Application Attack Surface

**Mobile app analysis:**
```
App: Example Corp Mobile App (iOS/Android)

API Endpoints:
- https://api.example.com/mobile/v1/*
- https://api-mobile.example.com/*

Authentication:
- OAuth 2.0 with JWT tokens
- Token storage: Keychain (iOS), KeyStore (Android)
- Token refresh mechanism observed

Hardcoded Secrets (via static analysis):
- API key found in decompiled code (MEDIUM risk)
- Analytics keys (LOW risk)
- No database credentials found (good)

Network Traffic (via proxy):
- Certificate pinning: NOT IMPLEMENTED (MitM risk)
- API calls over HTTPS (good)
- Some cleartext HTTP calls to analytics (data leakage)

Permissions Requested:
- Camera (for profile photos - justified)
- Location (for feature X - justify with user)
- Contacts (not clearly justified - excessive?)
- Storage (for document uploads - justified)

Data Storage:
- Local SQLite database (analyze if rooted/jailbroken)
- SharedPreferences/UserDefaults (check for sensitive data)
- Cache files (check for PII)

Deep Links:
- example://auth/callback (OAuth redirect - check for redirect vuln)
- example://share/{id} (check for IDOR)

Attack Vectors:
- Man-in-the-middle (no cert pinning)
- API key extraction (hardcoded)
- Local data access (if device compromised)
- Deep link manipulation
```

## Internal Attack Surface

Model what happens after initial compromise.

### Post-Compromise Entry Points

**Once inside the network:**
```
Scenario: Attacker gains VPN access via compromised credentials

Accessible Internal Systems:
- Internal wiki (corporate.internal)
- Jenkins CI/CD (jenkins.internal)
- GitLab (gitlab.internal)
- JIRA (jira.internal)
- Confluence (confluence.internal)
- Internal API gateway (api-internal.example.com)

Network Segmentation:
- Flat network observed (WEAK)
- Workstation VLAN can reach server VLAN
- No micro-segmentation

Attack Paths:
1. VPN -> Workstation network -> File shares -> Domain controller
2. VPN -> Jenkins -> Production servers (via deployment keys)
3. VPN -> GitLab -> Source code + secrets in repos
4. VPN -> Confluence -> Passwords in documents
```

### Lateral Movement Opportunities

**Pivot points:**
```
High-Value Pivot Assets:

Jenkins Server (jenkins.internal):
- Has SSH keys to production servers
- AWS credentials in environment variables
- Can deploy to production
- Access = production compromise

GitLab Server (gitlab.internal):
- Contains deployment keys
- CI/CD secrets
- API tokens for cloud providers
- Personal access tokens

Jump/Bastion Hosts:
- SSH keys to internal systems
- Compromise = full internal access

Domain Controller:
- Compromise = full Active Directory control
- Access to all domain-joined systems
```

### Privilege Escalation Paths

**Common escalation routes:**
```
User -> Admin Escalation:

Path 1: Service Account Abuse
- Service accounts with admin rights
- Weak passwords or stored credentials
- Kerberoasting opportunities

Path 2: Misconfigured Permissions
- Users with sudo/admin rights they don't need
- Overly permissive file shares
- Weak ACLs on critical systems

Path 3: Vulnerable Software
- Outdated internal applications
- Unpatched systems
- Known privilege escalation exploits

Path 4: Credential Harvesting
- Passwords in file shares
- Credentials in wikis/Confluence
- Browser password dumps
- Memory dumps (mimikatz)
```

## Human Attack Surface

People are often easier than technology.

### Email-Based Attack Surface

**Phishing targets:**
```
High-Value Targets:

Executives:
- CEO, CFO, CTO (business email compromise)
- Authority to approve wire transfers
- Access to strategic information

Finance Team:
- Process payments
- Access to financial systems
- BEC primary targets

IT/Security:
- Admin credentials
- VPN access
- Password reset capabilities

Engineering:
- Source code access
- Production system access
- Deployment capabilities

Support/Help Desk:
- Password reset authority
- Social engineering weak point
```

**Email infrastructure vulnerabilities:**
```
Email Security Analysis:

SPF Record: Includes google.com and sendgrid.net (~all)
- Soft fail (~all) vs hard fail (-all)
- Allows some spoofing potential

DMARC Policy: p=none
- Not enforcing
- Spoofed emails not blocked
- Recommendation: Move to p=quarantine then p=reject

DKIM: Configured
- Signing outbound email (good)

Email Gateway:
- Google Workspace with default settings
- Additional security: Proofpoint (observed in headers)
- Phishing protection: Basic (could be enhanced)

Internal Email Patterns:
- firstname.lastname@example.com
- Predictable, easy to spoof displays

Risk: Moderate to High for phishing/BEC
```

### Social Engineering Vectors

**Information available for pretexting:**
```
Public Information About Employees:

From LinkedIn:
- Names, titles, responsibilities
- Organizational structure
- Technologies they work with
- Office locations
- Phone numbers (sometimes)

From Company Website:
- Team photos and names
- Department structure
- Contact information

From Social Media:
- Personal interests
- Family information
- Location check-ins
- Routine patterns

Pretexting Scenarios:
- "IT support" password reset
- "Manager" urgent wire transfer
- "Vendor" invoice payment
- "New employee" access request
- "Remote employee" VPN issues
```

**Phone-based attacks:**
```
Published Phone Numbers:
- Main corporate line
- Support line
- Sales numbers
- Direct lines for executives (LinkedIn, news articles)

Vishing Vectors:
- Call support desk posing as employee
- CEO fraud (spoofed caller ID)
- Tech support scam (reverse)
- Vendor impersonation
```

### Physical Attack Surface

**Office locations:**
```
Physical Locations:
- HQ: 123 Main St, City, State
- Satellite office: [address]
- Remote workers: Distributed

Physical Access Points:
- Main entrance (badge required)
- Loading dock (less monitored?)
- Tailgating opportunities
- Visitor policy and enforcement

Physical Security Observations:
- Badge required for entry (observed)
- Security guard at HQ (yes)
- Visitor sign-in process
- Badge visible or hidden?

Physical Attack Vectors:
- Tailgating
- Fake badge
- Dumpster diving (trash/recycling)
- Network drops in public areas
- Unlocked workstations
- Shoulder surfing
```

## Supply Chain Attack Surface

Third parties extend your attack surface.

### Vendor Access Mapping

**Vendors with system access:**
```
Critical Vendors:

Cloud Providers (AWS, Azure, GCP):
- Admin access to infrastructure
- Console access
- API access
- Compromise impact: CRITICAL

SaaS Providers:
- Salesforce (customer data)
- GitHub (source code)
- Google Workspace (email, documents)
- Slack (communications)
- Each has admin accounts and API access

MSPs and Consultants:
- VPN access
- Admin privileges
- Support portal access

Payment Processors:
- Stripe, PayPal
- Access to payment flow
- Webhooks to internal systems

Third-Party Developers:
- Contractors with code access
- Temporary credential access
- May have persistent access after contract ends
```

**Vendor security posture:**
```
Vendor Risk Assessment:

For Each Vendor:
- Their security practices (SOC2? ISO27001?)
- Data access scope
- Authentication methods (SSO? MFA required?)
- Credential management
- Access logging and monitoring
- Incident response capability
- Breach history

High-Risk Vendors:
- Those with broad access
- Weak security posture
- History of breaches
- Minimal oversight
```

### Software Supply Chain

**Dependency analysis:**
```
Open Source Dependencies:

JavaScript (package.json):
- Total dependencies: 847 (direct + transitive)
- Known vulnerabilities: 13 (npm audit)
- Outdated packages: 156
- Unused dependencies: 23 (bloat)

Python (requirements.txt):
- Dependencies: 92
- Known vulnerabilities: 3 (safety check)

Risk Areas:
- Typosquatting (similar package names)
- Malicious packages (supply chain injection)
- Unmaintained packages (no security updates)
- Excessive dependencies (large attack surface)

Mitigation Checks:
- Dependency pinning: Partial (some ranges)
- Lock files: Present (good)
- Automated scanning: Not observed in CI
- SBOM generation: Not implemented
```

**Build pipeline security:**
```
CI/CD Analysis:

GitHub Actions Workflows:
- Runs on: ubuntu-latest (public runners)
- Secrets management: GitHub Secrets
- Third-party actions used: 15
- Unverified actions: 7 (risk)

Security Concerns:
- Public runners (supply chain risk)
- Third-party actions (malicious action risk)
- Secrets accessible in workflow (exposure risk)
- No workflow signing/verification

Jenkins (if applicable):
- Plugin security (outdated plugins?)
- Job configuration (who can modify?)
- Credential storage (encrypted?)
```

### Integration Points

**Third-party integrations:**
```
OAuth-Connected Services:

Application: app.example.com
Connected Services:
- Google (OAuth) - Read email, calendar
- Microsoft (OAuth) - Read email
- Salesforce (OAuth) - Full access
- Stripe (API key) - Payment processing
- SendGrid (API key) - Email sending
- Twilio (API key) - SMS sending
- AWS (IAM keys) - Infrastructure

Risk Assessment:
- Over-privileged OAuth scopes (Google has more access than needed)
- Long-lived API keys (no rotation observed)
- Stored in code repositories (some found in old commits)
- Insufficient access review (are all integrations still needed?)

Webhook Endpoints:
- /webhooks/stripe (payment events)
- /webhooks/github (code events)
- /webhooks/sendgrid (email events)

Webhook Security:
- Signature verification: Implemented (good)
- Rate limiting: Not observed (DDoS risk)
- Input validation: Unknown (injection risk?)
```

## Attack Path Modeling

Map specific attack paths from entry to objective.

### Attack Tree Example

```
Objective: Steal Customer Database

Path 1: Direct Database Access
- Entry: Exploit web application SQLi
- Privilege: Web app database user
- Escalate: Exploit database to OS command execution
- Pivot: N/A (direct access to database)
- Exfiltrate: Database dump over internet

Path 2: Compromised Employee
- Entry: Phish engineering employee
- Privilege: Employee workstation access
- Pivot: Workstation -> VPN -> Internal network
- Escalate: Harvest credentials from file shares
- Pivot: Internal network -> Database server
- Exfiltrate: Database dump via established C2

Path 3: Supply Chain
- Entry: Compromise third-party npm package
- Privilege: Code execution in application
- Escalate: Application environment variables (DB creds)
- Pivot: Direct database connection from app server
- Exfiltrate: Database dump via backdoor code

Path 4: Cloud Misconfiguration
- Entry: Enumerate S3 buckets
- Discover: example-com-backups (public read)
- Exfiltrate: Download database backup directly

Path 5: Vendor Compromise
- Entry: Compromise MSP credentials
- Privilege: MSP VPN access
- Pivot: VPN -> Admin network
- Escalate: MSP has domain admin rights
- Pivot: Domain admin -> Database server
- Exfiltrate: Database dump via admin access
```

Shortest path wins (for the attacker).

### Kill Chain Mapping

Map attacks to the cyber kill chain:

```
Kill Chain: Ransomware Deployment

1. Reconnaissance:
   - Scan for exposed RDP (3389)
   - Enumerate user accounts
   - Identify backup locations

2. Weaponization:
   - Prepare ransomware payload
   - Configure command & control

3. Delivery:
   - Brute force RDP credentials
   - OR phishing email with malicious attachment

4. Exploitation:
   - Execute payload on compromised system
   - OR exploit vulnerability for code execution

5. Installation:
   - Install persistence mechanisms
   - Disable antivirus and security tools
   - Deploy ransomware binary

6. Command & Control:
   - Establish C2 channel
   - Receive additional payloads/commands

7. Actions on Objectives:
   - Lateral movement to file servers and backups
   - Delete/encrypt backups
   - Encrypt production data
   - Display ransom note
```

## Attack Surface Reduction Opportunities

Identify areas to reduce attack surface.

**Quick wins:**
- Close exposed database port on legacy.example.com
- Fix S3 bucket permissions (example-com-backups)
- Remove public access to API documentation
- Enable DMARC enforcement (p=quarantine)
- Implement certificate pinning in mobile app
- Remove unused subdomains

**Medium effort:**
- Implement rate limiting on authentication endpoints
- Enforce MFA for all users
- Segment internal network
- Review and reduce OAuth permissions
- Audit and remove unused cloud resources

**Long-term:**
- Zero trust architecture implementation
- Privileged access management (PAM)
- Vendor risk management program
- Supply chain security improvements

## Documentation Outputs

### Attack Surface Inventory

```
Category: External Web Applications
Entry Points: 12
Critical: 2
High: 6
Medium: 3
Low: 1

Category: APIs
Endpoints: 47
Unauthenticated: 8
Authenticated: 39
Admin: 6

Category: Exposed Services
Services: 15
Critical: 1 (MySQL on legacy host)
High: 2
Medium: 8
Low: 4

Category: Cloud Resources
S3 Buckets: 8
Public: 3 (1 intended, 2 misconfigured)
Private: 5

Category: Human Targets
Employees: ~500
High-value: 23 (executives, finance, IT)
```

### Attack Path Documentation

For each critical asset, document viable attack paths with likelihood and difficulty.

### Findings Summary

Immediate attention:
- MySQL exposed to internet (legacy.example.com:3306)
- S3 buckets with sensitive data publicly accessible
- No MFA enforcement
- Weak network segmentation

Quick fixes:
- Close database port
- Fix S3 permissions
- Enable DMARC enforcement
- Remove unused subdomains

## Next Steps

Attack surface mapped. Every entry point documented. Every attack path identified.

Chapter 6 covers applying formal threat modeling methodologies (STRIDE, PASTA, etc.) to build comprehensive threat scenarios using the attack surface analysis.

You know the routes in. Now model the specific threats taking those routes.