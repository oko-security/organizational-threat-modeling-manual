# Chapter 4: Threat Identification

## Overview

You know what assets exist. Now identify who wants them and why.

Threat identification is about understanding adversaries - their motivations, capabilities, and typical approaches. Different threat actors target different assets using different methods. A script kiddie and a nation-state may target the same organization, but the threats they pose are completely different.

This chapter covers threat actor profiling, threat intelligence integration, and mapping specific threats to your asset inventory.

## Threat Actor Categories

### External Threat Actors

**Opportunistic Attackers (Script Kiddies):**

Characteristics:
- Low skill level
- Use automated tools and known exploits
- No specific target selection
- Mass scanning and exploitation
- Looking for easy wins

Motivations:
- Recognition and reputation
- Learning and experimentation
- Minor financial gain
- Chaos and disruption

Typical targets:
- Unpatched systems
- Default credentials
- Known vulnerabilities
- Misconfigurations
- Anything exposed and vulnerable

Capabilities:
- Automated scanning tools (Shodan, Censys)
- Public exploit code (Exploit-DB, Metasploit)
- Credential stuffing tools
- Basic social engineering
- Simple malware deployment

Likelihood: High
Organizations are constantly scanned by automated tools.

**Financially Motivated Criminals:**

Characteristics:
- Moderate to high skill level
- Organized groups or individuals
- ROI-focused target selection
- Persistence until profitable
- Business-like operations

Motivations:
- Direct financial gain
- Monetizable data theft
- Ransomware payments
- Fraud and theft
- Cryptocurrency mining

Typical targets:
- Payment systems
- Customer databases (for sale or fraud)
- Business email accounts (BEC)
- Any system for ransomware
- Cryptocurrency wallets and exchanges
- High-value individual accounts

Capabilities:
- Custom malware development
- Social engineering and phishing
- Exploit development (for high-value targets)
- Ransomware deployment
- Data exfiltration and sale
- Money laundering infrastructure

Likelihood: Moderate to High
Depends on industry and public profile.

**Nation-State Actors (APTs):**

Characteristics:
- Highly skilled and well-resourced
- Long-term persistent campaigns
- Strategic target selection
- Stealth and anti-forensics focus
- Unlimited time and budget

Motivations:
- Espionage (political, economic, military)
- Strategic advantage
- Intellectual property theft
- Critical infrastructure disruption
- Pre-positioning for future operations

Typical targets:
- Government contractors
- Critical infrastructure
- Telecommunications
- Technology companies
- Research institutions
- High-value intellectual property
- Strategic business intelligence

Capabilities:
- Zero-day exploits
- Supply chain compromise
- Advanced persistent access
- Custom malware and tooling
- Physical access operations
- Insider recruitment
- Sophisticated social engineering

Likelihood: Low to Moderate
Depends on industry, geopolitical relevance, and strategic value.

**Hacktivists:**

Characteristics:
- Variable skill levels
- Ideologically motivated
- Public campaigns and messaging
- Reputational damage focus
- Short to medium-term operations

Motivations:
- Political statements
- Social justice causes
- Environmental activism
- Anti-corporate sentiment
- Exposure of wrongdoing

Typical targets:
- Organizations with controversial policies
- Government agencies
- Corporations in specific industries
- High-profile targets for maximum impact

Capabilities:
- DDoS attacks
- Website defacement
- Data breaches and leaks
- Social media hijacking
- Social engineering
- Collaboration with insiders

Likelihood: Low to Moderate
Depends on organizational profile and public perception.

**Competitors (Corporate Espionage):**

Characteristics:
- Well-funded and skilled
- Specific intelligence objectives
- Long-term covert operations
- Economic advantage focus

Motivations:
- Intellectual property theft
- Business intelligence
- Strategic advantage
- Product development shortcuts
- Competitive bidding information

Typical targets:
- R&D departments
- Product roadmaps
- Customer lists and contracts
- Pricing strategies
- M&A plans
- Executive communications

Capabilities:
- Insider recruitment
- Social engineering
- Targeted phishing
- Supply chain infiltration
- Economic resources for sophisticated attacks

Likelihood: Low to Moderate
Higher in competitive industries (tech, pharma, defense).

### Internal Threat Actors

**Malicious Insiders:**

Characteristics:
- Legitimate access and knowledge
- Trusted position within organization
- Understanding of security controls
- Ability to operate covertly

Motivations:
- Financial gain (selling data)
- Revenge or grievance
- Ideology or allegiance
- Coercion or recruitment
- Personal gain

Typical targets:
- Sensitive data accessible to their role
- Systems they regularly use
- Financial systems
- Intellectual property
- Customer data

Capabilities:
- Authorized access to systems
- Knowledge of security controls
- Understanding of data locations
- Ability to bypass monitoring
- Physical access

Likelihood: Low
But impact can be severe.

**Negligent Insiders:**

Characteristics:
- Unintentional security failures
- Lack of awareness or training
- Convenience over security mindset
- No malicious intent

Common behaviors:
- Clicking phishing links
- Using weak passwords
- Sharing credentials
- Misconfiguring systems
- Losing devices
- Shadow IT usage
- Circumventing security controls

Capabilities:
- Inadvertent data exposure
- Credential compromise
- Malware introduction
- Configuration errors
- Unintentional insider threat

Likelihood: High
Human error is constant.

**Compromised Insiders:**

Characteristics:
- Legitimate user whose credentials are stolen
- User may not know they're compromised
- Attacker operates with user's privileges

How compromise occurs:
- Phishing
- Credential stuffing
- Password reuse
- Malware infection
- Session hijacking

Capabilities:
- All capabilities of the compromised user
- Lateral movement
- Data access within user's permissions
- Persistence through legitimate access

Likelihood: Moderate
Depends on security awareness and controls.

### Third-Party Threat Vectors

**Compromised Vendors:**

Risk:
- Vendor systems compromised
- Attacker pivots to customer environments
- Shared infrastructure exploitation
- Supply chain attack

Examples:
- SolarWinds compromise
- Kaseya ransomware
- Codecov breach
- Cloud provider vulnerabilities

**Malicious Vendors:**

Risk:
- Intentionally malicious software or services
- Backdoors in products
- Data exfiltration through legitimate tools

**Negligent Vendors:**

Risk:
- Poor vendor security practices
- Data exposure through vendor systems
- Inadequate access controls
- Weak authentication

## Threat Actor Profiling

For each threat actor relevant to the organization, create a profile.

### Threat Actor Profile Template

```
Threat Actor: Advanced Persistent Threat (APT) Group - Hypothetical Tech Sector Focus

Classification: Nation-State Sponsored
Sophistication Level: Advanced
Resource Level: High (state-funded)

Motivations:
- Intellectual property theft
- Technology transfer
- Economic espionage
- Strategic intelligence gathering

Objectives:
- Steal product designs and source code
- Obtain customer lists and contracts
- Access strategic business plans
- Maintain long-term persistent access

Typical TTPs (Tactics, Techniques, Procedures):
- Spear phishing targeting engineers and executives
- Watering hole attacks on industry sites
- Zero-day exploits in common software
- Supply chain compromise
- Living-off-the-land techniques
- Custom malware with anti-forensics
- Long-term persistence (months to years)

Target Selection:
- Technology companies with valuable IP
- Government contractors
- Research institutions
- Companies in strategic sectors

Entry Vectors:
- Spear phishing emails
- Compromised vendor access
- Exploitation of VPN or remote access
- Zero-day exploitation
- Insider recruitment

Indicators:
- Off-hours access from unusual locations
- Large data transfers to external IPs
- Access to unrelated systems (lateral movement)
- Use of legitimate credentials in suspicious patterns
- Anti-forensic activities (log deletion, etc.)

Relevant to Organization: [Yes/No - explain why]

Likelihood of Targeting: [Low/Moderate/High]
Based on: Industry sector (technology), IP value (high), geopolitical factors

Assets of Interest:
- Source code repositories
- Product development systems
- Customer databases
- Executive email accounts
- Strategic planning documents
- R&D data
```

Create profiles for each relevant threat actor category.

## Industry-Specific Threats

Different industries face different threat landscapes.

### Financial Services

Primary threats:
- Financially motivated criminals (fraud, theft)
- Nation-states (economic intelligence)
- Insiders (data theft, fraud)

Common attack vectors:
- Business email compromise
- Wire transfer fraud
- Account takeover
- Card skimming and fraud
- Ransomware

Valuable assets:
- Customer financial data
- Transaction systems
- Account credentials
- Trading algorithms
- M&A information

### Healthcare

Primary threats:
- Ransomware operators
- Data thieves (PHI has black market value)
- Nation-states (medical research, COVID intelligence)

Common attack vectors:
- Ransomware
- Phishing
- Unpatched medical devices
- Third-party vendor compromise

Valuable assets:
- Patient health records
- Billing systems
- Medical devices
- Research data
- Prescription systems

### Technology and SaaS

Primary threats:
- Nation-states (IP theft)
- Competitors (corporate espionage)
- Criminals (ransomware, data theft)
- Supply chain attackers

Common attack vectors:
- Supply chain compromise
- Source code theft
- Customer data breaches
- Infrastructure compromise
- Zero-day exploitation

Valuable assets:
- Source code
- Customer data
- Product roadmaps
- Proprietary algorithms
- Cloud infrastructure

### Retail and E-Commerce

Primary threats:
- Financially motivated criminals
- Payment card fraudsters
- Data brokers

Common attack vectors:
- Payment card skimming (Magecart)
- Credential stuffing
- Account takeover
- POS malware
- Web application attacks

Valuable assets:
- Payment card data
- Customer accounts
- Loyalty program data
- Transaction systems
- Inventory systems

### Critical Infrastructure

Primary threats:
- Nation-states
- Hacktivists
- Terrorists (low probability, high impact)

Common attack vectors:
- Supply chain compromise
- ICS/SCADA exploitation
- Insider threats
- Physical and cyber combined attacks

Valuable assets:
- SCADA systems
- Control systems
- Operational technology
- Safety systems
- Physical infrastructure

### Government and Defense

Primary threats:
- Nation-states
- Terrorists
- Hacktivists

Common attack vectors:
- Spear phishing
- Zero-day exploits
- Insider threats
- Physical access

Valuable assets:
- Classified information
- PII of citizens or employees
- Operational systems
- Communication systems

## Threat Intelligence Integration

Don't work in a vacuum. Use threat intelligence to inform threat modeling.

### Public Threat Intelligence Sources

**MITRE ATT&CK:**
Framework of adversary tactics and techniques.
- Map known threat actor TTPs
- Understand attack patterns
- Identify gaps in detection

**CISA Alerts:**
US government threat warnings and indicators.
- Current threat campaigns
- Vulnerability alerts
- Sector-specific threats

**Vendor Threat Reports:**
- CrowdStrike, Mandiant, Palo Alto Unit 42
- Industry-specific intelligence
- Threat actor profiles
- IOCs and TTPs

**Information Sharing Groups:**
- ISACs (Information Sharing and Analysis Centers)
- Industry-specific intelligence sharing
- Sector threat briefings

**Open Source Intelligence:**
- News reports of breaches
- Security researcher disclosures
- Conference presentations
- Blog posts and writeups

### Commercial Threat Intelligence

**Threat Intel Platforms:**
- Recorded Future
- ThreatConnect
- Anomali
- Flashpoint

Benefits:
- Automated indicator matching
- Threat actor tracking
- Predictive intelligence
- Dark web monitoring

**Dark Web Monitoring:**
- Credential leaks
- Data sales
- Attack planning discussions
- Exploit development

### Applying Threat Intelligence

**Map threats to assets:**
```
Threat: Magecart Payment Skimming

Relevant Assets:
- A012: E-commerce checkout page
- A045: Payment processing form
- A089: Third-party payment widget

Attack Pattern:
- JavaScript injection into checkout process
- Keylogging payment card data
- Exfiltration to attacker-controlled domain

Indicators:
- Unexpected JavaScript includes
- External script sources
- Data exfiltration to unusual domains
- Modified checkout page code

Mitigations:
- Content Security Policy (CSP)
- Subresource Integrity (SRI)
- JavaScript integrity monitoring
- Third-party script review
```

**Track relevant threat actors:**
Maintain a watchlist of threat actors targeting your industry or organization type.

**Monitor for targeting indicators:**
- Reconnaissance activity (scanning, probing)
- Spear phishing campaigns
- Mentions on dark web forums
- Credential leaks from breaches

## Historical Incident Analysis

Learn from past incidents - yours and others'.

### Organizational History

**Previous incidents:**
If the organization has experienced breaches or attacks:
- What happened?
- How did attackers get in?
- What was compromised?
- What was the impact?
- What controls failed?
- What's been fixed?
- What hasn't been fixed?

Use this to prioritize threats. If they got phished before, phishing is a high-priority threat.

### Peer Incidents

**Similar organizations:**
Look for publicized breaches of similar companies:
- Same industry
- Similar size
- Similar tech stack
- Geographic region

Extract patterns:
- Common attack vectors
- Exploited vulnerabilities
- Attacker objectives
- Impact and consequences

### Recent Trends

**Current threat landscape:**
What's happening now?
- Ransomware surge (2020-present)
- Supply chain attacks (SolarWinds era)
- Cloud misconfigurations (ongoing)
- Zero-day exploitation waves
- Cryptocurrency-related attacks

Adjust threat priorities based on current activity.

## Mapping Threats to Assets

Connect specific threats to specific assets.

### Threat-Asset Matrix

```
Asset: A047 - api.example.com (Customer API)

Threat 1: Credential Stuffing
- Threat Actor: Financially motivated criminals
- Entry Point: Login endpoint (/api/auth/login)
- Attack Vector: Stolen credentials from breaches
- Objective: Account takeover, data theft
- Likelihood: High (common, easy to execute)
- Impact: High (customer data compromise)

Threat 2: API Abuse
- Threat Actor: Competitors, data scrapers
- Entry Point: Public API endpoints
- Attack Vector: Automated data collection
- Objective: Data harvesting, competitive intelligence
- Likelihood: Moderate (requires some effort)
- Impact: Moderate (data exposure, service degradation)

Threat 3: SQL Injection
- Threat Actor: Opportunistic attackers, criminals
- Entry Point: API parameters and queries
- Attack Vector: Malicious input exploitation
- Objective: Database access, data theft
- Likelihood: Low (assuming modern framework with parameterized queries)
- Impact: Critical (database compromise)

Threat 4: DDoS
- Threat Actor: Competitors, hacktivists, extortionists
- Entry Point: Public API endpoints
- Attack Vector: Request flooding
- Objective: Service disruption, extortion
- Likelihood: Moderate (depends on profile)
- Impact: High (revenue loss, reputation)

Threat 5: Third-Party Compromise
- Threat Actor: Various (through compromised vendor)
- Entry Point: Integrated third-party services (Stripe, SendGrid)
- Attack Vector: Compromised API keys or vendor breach
- Objective: Data theft, service disruption
- Likelihood: Low (but increasing)
- Impact: High (customer data, payment data)
```

Create this for each critical and high-value asset.

### Prioritization

Not all threats are equally likely or impactful.

**High priority threats:**
- High likelihood AND high impact
- Currently trending in threat landscape
- Previously successful against the organization
- Low cost for attacker, high cost for defender

**Medium priority threats:**
- Moderate likelihood or impact
- Industry-relevant but not currently active
- Require some skill or resources

**Low priority threats:**
- Low likelihood AND/OR low impact
- Require significant resources
- Theoretical or uncommon

## Insider Threat Considerations

Insiders have advantages attackers don't.

### Insider Advantages

**Legitimate access:**
- Already authenticated
- Authorized to access systems
- Bypass perimeter security
- Trusted by systems and people

**Knowledge:**
- Understand architecture
- Know where valuable data is
- Aware of security controls
- Understand normal operations

**Time:**
- Can operate slowly to avoid detection
- Access over extended periods
- No rush to monetize immediately

**Physical access:**
- On-site access to systems
- Physical device access
- Ability to plant hardware

### Insider Threat Scenarios

**Data exfiltration:**
- Copy customer database to USB drive
- Email sensitive documents to personal account
- Upload source code to personal cloud storage
- Clone repositories to external systems

**Sabotage:**
- Delete critical data
- Modify code to introduce backdoors
- Disable security controls
- Corrupt databases or systems

**Fraud:**
- Manipulate financial transactions
- Create fake accounts or users
- Alter records for personal gain

**Espionage:**
- Steal IP for competitor
- Provide access to external attacker
- Exfiltrate strategic business plans

### Detection Considerations

Insider threats are harder to detect:
- Legitimate credentials used
- Normal access patterns (to a point)
- No external network signatures
- May have knowledge of monitoring

Indicators:
- Off-hours access
- Access to unusual systems
- Large data transfers
- Accessing unrelated data
- Attempts to access restricted systems
- Disabling logging or monitoring
- Privilege escalation attempts
- Resignation or termination proximity

## Emerging Threats

Stay current with evolving threat landscape.

### AI and Machine Learning Threats

**AI-powered attacks:**
- Automated vulnerability discovery
- AI-generated phishing (more convincing)
- Deepfake social engineering
- Adaptive malware

**Machine learning poisoning:**
- Training data manipulation
- Model theft
- Adversarial examples

### Cloud-Native Threats

**Container attacks:**
- Container escape
- Malicious images
- Orchestrator compromise (Kubernetes)

**Serverless attacks:**
- Function injection
- Event manipulation
- Over-privileged functions

**Cloud misconfigurations:**
- Public S3 buckets (ongoing)
- Exposed credentials in infrastructure-as-code
- Over-permissive IAM policies

### Supply Chain Evolution

**Dependency attacks:**
- Malicious packages (npm, PyPI)
- Typosquatting
- Package takeover
- Compromised build pipelines

**SaaS supply chain:**
- OAuth permission abuse
- Integration compromise
- Third-party data access

### IoT and Edge

**IoT botnets:**
- Default credentials
- Unpatched devices
- DDoS amplification

**Edge computing attacks:**
- Compromised edge nodes
- Data interception
- Physical access risks

## Threat Modeling Output

### Deliverables

**Threat actor profiles:**
Document for each relevant threat actor category.

**Threat-asset mapping:**
Spreadsheet or database linking threats to specific assets.

**Attack scenario library:**
Detailed scenarios for high-priority threats.

**Threat prioritization matrix:**
Threats ranked by likelihood and impact.

**Intelligence summary:**
Current threat landscape relevant to organization.

**Historical context:**
Lessons from past incidents (organizational and industry).

### Sample Threat Summary

```
Organization: Example Corp
Industry: SaaS/Technology
Date: 2025-12-23

Top Threat Actors:
1. Financially Motivated Criminals (HIGH)
   - Ransomware deployment
   - Customer data theft and sale
   - Business email compromise

2. Opportunistic Attackers (HIGH)
   - Automated vulnerability exploitation
   - Credential stuffing
   - Web application attacks

3. Nation-State APTs (MODERATE)
   - IP theft (source code, algorithms)
   - Customer intelligence gathering
   - Long-term persistent access

4. Competitors (MODERATE)
   - Corporate espionage
   - Customer list theft
   - Product roadmap intelligence

5. Insiders (LOW-MODERATE)
   - Negligent data exposure
   - Accidental misconfiguration
   - Potential malicious actors

Critical Threats Identified: 47
High Priority Threats: 23
Medium Priority Threats: 38
Low Priority Threats: 52

Most Likely Attack Vectors:
1. Phishing and social engineering
2. Unpatched vulnerabilities
3. Credential stuffing
4. Cloud misconfigurations
5. Third-party compromise

Assets at Greatest Risk:
- A047: Customer API (credential stuffing, DDoS)
- A023: Customer database (data breach, ransomware)
- A156: Admin panel (unauthorized access)
- A089: GitHub org (IP theft, credential exposure)
- A234: Email system (phishing, BEC)

Intelligence Highlights:
- Recent uptick in ransomware targeting SaaS companies
- Credential stuffing campaigns observed in industry
- Supply chain attacks increasing (monitor dependencies)
- APT group [name] actively targeting tech sector

Recommended Focus Areas:
- Email security and phishing resistance
- API authentication and rate limiting
- Database backup and encryption
- Access control and privilege management
- Vendor risk management
```

## Next Steps

Threat identification complete. You know who the adversaries are and what they want.

Chapter 5 covers attack surface analysis - mapping exactly how those threat actors could reach your assets. It's one thing to know ransomware is a threat. It's another to map the specific paths an attacker could take to deploy it.

The threats are identified. Now map the routes.