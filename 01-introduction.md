# Chapter 1: Introduction and Methodology

## What is Organizational Threat Modeling

Organizational threat modeling is the systematic process of identifying, understanding, and prioritizing security threats to an entire company's digital footprint. Unlike application-specific threat modeling, this approach examines the organization as a complete system - every public asset, every employee, every vendor relationship, every piece of infrastructure.

The goal is simple: understand what you have, who wants it, and how they might get it.

This differs from penetration testing or vulnerability assessment. You're not exploiting systems. You're building a comprehensive understanding of risk across the entire organization. Think of it as creating a threat map rather than testing defenses.

## When to Perform Threat Modeling

**Initial assessment scenarios:**
- New client onboarding for security consulting
- Post-acquisition security review
- Pre-IPO security posture evaluation
- Compliance requirement (SOC2, ISO27001)
- Major infrastructure changes
- After a security incident

**Recurring assessment triggers:**
- Quarterly or annual security reviews
- New product or service launches
- Significant organizational changes
- Major vendor or partnership additions
- Regulatory requirement updates
- Threat landscape shifts

**Time investment:**
Small organization (under 100 employees): 40-80 hours
Medium organization (100-1000 employees): 80-160 hours
Large organization (1000+ employees): 160-400+ hours

These estimates include discovery, analysis, and documentation. Complex environments with multiple cloud providers, extensive SaaS usage, or highly regulated industries will take longer.

## Scope Definition and Boundaries

Define what's in scope before you start. Scope creep kills timelines and budgets.

### Typical Scope Elements

**In scope:**
- All domains owned by the organization
- Public-facing infrastructure and applications
- Cloud resources (AWS, Azure, GCP, etc.)
- Employee-facing systems accessible externally
- Public code repositories
- Corporate email infrastructure
- Third-party integrations with data access
- Mobile applications
- Physical security touchpoints (if relevant)

**Typically out of scope:**
- Physical penetration testing
- Social engineering testing (unless explicitly included)
- Offensive security testing (exploitation)
- Internal network assessment (unless network diagrams provided)
- Source code review (unless repositories are public or provided)
- Vendor-managed infrastructure (SaaS backends)

**Define boundaries:**
- Geographic limitations (specific regions only)
- Technical limitations (specific systems only)
- Time limitations (discovery period, not continuous)
- Data limitations (public information only vs. provided access)

### Scope Documentation Template

```
Organization: [Company Name]
Assessment Period: [Start Date] - [End Date]
Assessment Type: External Threat Modeling

In Scope:
- Primary domain: example.com
- Subsidiary domains: subsidiary1.com, subsidiary2.com
- Cloud infrastructure: AWS (all regions), Azure (North America)
- Public repositories: github.com/example-org
- Employee count: ~500 (for social engineering surface assessment)
- Third-party integrations: [List major vendors]

Out of Scope:
- Internal network (no access provided)
- Physical locations
- Active exploitation or vulnerability testing
- Recently acquired company (acquisition-corp.com) - separate assessment

Assumptions:
- Public information only (no insider access)
- No authentication credentials provided
- Discovery based on OSINT methods
- No disruption to production systems

Deliverables:
- Comprehensive threat model document
- Asset inventory spreadsheet
- Risk assessment matrix
- Executive presentation
- Remediation roadmap
```

## Legal and Ethical Considerations

Do not skip this section. It keeps you out of legal trouble.

### Legal Boundaries

**What is legal:**
- Searching public records and databases
- DNS enumeration and subdomain discovery
- Port scanning (generally, but check local laws)
- Reading public code repositories
- Certificate transparency log analysis
- Social media and LinkedIn research
- Public document discovery
- Analyzing public web applications (read-only)

**What is not legal (without permission):**
- Unauthorized access to systems
- Exploitation of vulnerabilities
- Bypassing authentication or authorization
- Denial of service testing
- Accessing private data or systems
- Impersonation or social engineering
- Breaking terms of service agreements

### Authorization Requirements

**Get it in writing:**
- Signed statement of work or engagement letter
- Explicit permission for reconnaissance activities
- Clear scope boundaries documented
- Acknowledgment of methods to be used
- Contact information for technical escalation
- Emergency stop procedure

**Sample authorization clause:**
```
[Client Name] authorizes [Consultant/Firm Name] to perform
reconnaissance and threat modeling activities against the systems
and domains listed in the scope section. This authorization includes
subdomain enumeration, port scanning, technology fingerprinting, and
analysis of publicly accessible information.

This authorization does not include exploitation of vulnerabilities,
unauthorized access, or any activity that may disrupt normal business
operations.

Authorized by: [Name, Title]
Date: [Date]
```

### Ethical Guidelines

**Principles:**
- Do no harm - never disrupt or damage systems
- Respect privacy - limit collection to what's necessary
- Maintain confidentiality - protect client information
- Accurate reporting - don't exaggerate or minimize findings
- Professional conduct - no showing off or unnecessary risk-taking

**When you find something bad:**
If you discover an active breach or critical vulnerability during reconnaissance:
1. Document it immediately
2. Notify the client through established channels
3. Do not continue to explore the breach
4. Do not publicly disclose
5. Follow responsible disclosure practices

**Third-party data:**
If you discover sensitive information about the client on third-party sites (pastebin, breach databases):
1. Document the source and data
2. Do not download or store sensitive customer data
3. Record metadata (what was exposed, where, when)
4. Provide findings to client for their remediation

## Engagement Planning

Plan the work before you work the plan.

### Pre-Engagement Activities

**Client kickoff meeting:**
- Confirm scope and boundaries
- Identify stakeholders and points of contact
- Establish communication channels
- Set expectations for findings and reporting
- Discuss timeline and milestones
- Clarify deliverable formats

**Information gathering:**
- Primary domain names
- Known subsidiaries or acquisitions
- Major products or services
- Compliance requirements
- Previous security assessments (if available)
- Known incidents or concerns
- Organizational structure overview

**Tool preparation:**
- Set up reconnaissance infrastructure
- Prepare collection databases or spreadsheets
- Configure automation tools
- Set up secure storage for findings
- Prepare documentation templates

### Timeline Planning

**Phase-based approach:**

**Week 1-2: Discovery**
- Domain and subdomain enumeration
- Infrastructure mapping
- Initial technology fingerprinting
- Employee and org intelligence
- Code repository discovery

**Week 2-3: Deep Reconnaissance**
- Detailed technology stack analysis
- Third-party service mapping
- Historical data analysis
- Attack surface documentation
- Asset classification

**Week 3-4: Threat Modeling**
- Threat actor identification
- Attack vector mapping
- Scenario development
- Initial risk assessment

**Week 4-5: Analysis and Risk Assessment**
- Detailed risk scoring
- Prioritization
- Mitigation recommendations
- Cost-benefit analysis

**Week 5-6: Documentation and Delivery**
- Report writing
- Visual documentation
- Executive summary preparation
- Presentation development
- Client review and revision

Adjust based on organization size and complexity.

### Communication Plan

**Status updates:**
- Weekly status emails to primary contact
- Immediate notification of critical findings
- Milestone completion notifications
- Issue escalation path

**Documentation storage:**
- Secure file sharing (encrypted)
- Version control for deliverables
- Access controls for sensitive findings
- Retention and destruction policy

## Documentation Requirements

Document everything as you go. Trying to reconstruct your work later is painful.

### Real-Time Documentation

**Discovery logs:**
```
Date: 2025-12-23
Activity: Subdomain enumeration
Tool: subfinder
Command: subfinder -d example.com -o subdomains.txt
Results: 247 subdomains discovered
Notable findings: dev.example.com returns 200, staging.example.com exposed
Next steps: Technology fingerprinting on development environments
```

Keep timestamped notes of:
- Commands executed
- Tools used
- Results obtained
- Interesting findings
- Follow-up items

### Evidence Collection

**Screenshot policy:**
- Capture exposed admin panels
- Document error messages with info disclosure
- Record configuration exposures
- Save certificate details
- Document cloud storage misconfigurations

**Data retention:**
- Raw tool output files
- Curated findings lists
- Analysis notes
- Communication with client
- Version-controlled documentation

### Working Files

**Maintain organized structure:**
```
engagement-name/
├── 01-discovery/
│   ├── domains.txt
│   ├── subdomains.txt
│   ├── ip-ranges.txt
│   └── raw-output/
├── 02-assets/
│   ├── asset-inventory.xlsx
│   ├── technology-stack.md
│   └── network-diagram.png
├── 03-threats/
│   ├── threat-actors.md
│   ├── attack-scenarios.md
│   └── threat-matrix.xlsx
├── 04-risks/
│   ├── risk-assessment.xlsx
│   └── prioritization.md
├── 05-deliverables/
│   ├── executive-report.pdf
│   ├── technical-report.md
│   └── presentation.pptx
└── notes/
    └── daily-logs.md
```

## Methodology Overview

The methodology follows a logical progression:

**1. Reconnaissance (Chapter 2)**
Discover everything about the organization's public footprint.

**2. Asset Inventory (Chapter 3)**
Organize and classify what you found.

**3. Threat Identification (Chapter 4)**
Determine who might attack and why.

**4. Attack Surface Analysis (Chapter 5)**
Map how attackers could reach assets.

**5. Threat Modeling (Chapter 6)**
Apply frameworks to build threat scenarios.

**6. Risk Assessment (Chapter 7)**
Calculate and prioritize risks.

**7. Documentation (Chapter 8)**
Package findings for the client.

**8. Maintenance Planning (Chapter 9)**
Establish ongoing monitoring and updates.

Each phase feeds the next. You can't assess risk without understanding threats. You can't identify threats without knowing your assets. You can't catalog assets without discovery.

The order matters.

## Tools and Environment Setup

Set up your environment before starting.

### Required Tools

**Reconnaissance:**
- subfinder, amass, assetfinder (subdomain enumeration)
- nmap, masscan (network scanning)
- httpx, httprobe (HTTP probing)
- nuclei (vulnerability templates)
- whatweb (technology fingerprinting)

**OSINT:**
- theHarvester (email and employee enumeration)
- recon-ng (reconnaissance framework)
- spiderfoot (automation)
- maltego (visual reconnaissance)

**Analysis:**
- Spreadsheet software (Excel, Google Sheets)
- Diagram tools (draw.io, Lucidchart)
- Note-taking (Obsidian, Notion, Markdown editor)

**Installation:**
Most tools are available via package managers or GitHub. Set up a dedicated VM or container for reconnaissance work to keep tools organized and isolated.

### Data Organization

Use a consistent naming convention and structure. When you're dealing with hundreds of assets and thousands of data points, organization is critical.

Start with templates and fill them in as you work. Trying to organize everything at the end never works.

## Next Steps

With the methodology understood and engagement planned, you're ready to start reconnaissance. Chapter 2 covers the detailed discovery process.

The rest is just doing the work.