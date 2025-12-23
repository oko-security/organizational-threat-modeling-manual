# Chapter 3: Asset Inventory and Classification

## Overview

Raw reconnaissance data is overwhelming. Hundreds of subdomains, dozens of IP ranges, repositories full of code, employee lists - it's all just noise until you organize it.

This chapter covers transforming discovery output into a structured asset inventory. You need to know what you have, what it does, how critical it is, and how the pieces connect. This inventory becomes the foundation for threat modeling.

## Building the Asset Database

### Database Structure

Start with a structured format. Spreadsheets work fine for smaller engagements. Larger organizations may need a proper database.

**Core fields:**
```
Asset ID          Unique identifier (A001, A002, etc.)
Asset Type        Domain, Subdomain, IP, Repository, Employee, Service, etc.
Asset Name        Specific identifier (api.example.com, 203.0.113.10)
Description       What it does or purpose
Owner             Department or team responsible
Discovery Method  How you found it (subfinder, manual, certificate logs)
Discovery Date    When you discovered it
Status            Active, Inactive, Deprecated, Unknown
```

**Technical fields:**
```
Technology Stack  Software, frameworks, languages
Services/Ports    Running services (HTTP/443, SSH/22, etc.)
Cloud Provider    AWS, Azure, GCP, On-Premise, Unknown
Region            Geographic location or cloud region
IP Address        Associated IP(s)
DNS Records       A, CNAME, MX, etc.
SSL Certificate   Issuer, expiration, SANs
```

**Classification fields (covered in detail later):**
```
Criticality       Critical, High, Medium, Low
Data Sensitivity  Public, Internal, Confidential, Restricted
Business Impact   Revenue-generating, operational, support, etc.
Compliance Scope  PCI, HIPAA, SOC2, GDPR applicability
```

**Relationship fields:**
```
Dependencies      What this asset depends on
Dependents        What depends on this asset
Related Assets    Connected or similar assets
Data Flows        Where data comes from and goes to
```

### Sample Asset Inventory Entry

```
Asset ID: A047
Asset Type: Web Application
Asset Name: api.example.com
Description: Customer-facing REST API for mobile app
Owner: Engineering - API Team
Discovery Method: Subdomain enumeration (subfinder)
Discovery Date: 2025-12-20
Status: Active

Technology Stack:
  - Node.js 18.x
  - Express.js framework
  - PostgreSQL database
  - Redis cache
  - Nginx reverse proxy

Services/Ports:
  - HTTPS/443 (public)
  - HTTP/80 (redirects to HTTPS)

Cloud Provider: AWS
Region: us-east-1
IP Address: 54.210.123.45 (AWS)
DNS Records: CNAME to lb-api-prod-123456.us-east-1.elb.amazonaws.com

SSL Certificate:
  - Issuer: Let's Encrypt
  - Expiration: 2025-03-15
  - SANs: api.example.com

Criticality: Critical
Data Sensitivity: Confidential (customer PII, payment data)
Business Impact: Revenue-generating (mobile app depends on this)
Compliance Scope: PCI-DSS, SOC2, GDPR

Dependencies:
  - A023 (PostgreSQL RDS instance)
  - A089 (Redis ElastiCache cluster)
  - A112 (Auth service for token validation)

Dependents:
  - A003 (iOS mobile app)
  - A004 (Android mobile app)
  - A156 (Internal admin dashboard)

Data Flows:
  - Receives: User authentication tokens, API requests
  - Sends: Customer data, transaction records
  - Integrates: Stripe API, SendGrid API

Notes:
  - Rate limiting observed (100 req/min per IP)
  - Uses API keys in Authorization header
  - GraphQL endpoint also available at /graphql
  - Staging environment at api-staging.example.com
```

## Asset Categorization

Group assets into logical categories for easier analysis.

### Infrastructure Assets

**Domains and Subdomains:**
- Corporate website domains
- Product domains
- Regional variants
- Functional subdomains (mail, vpn, api, etc.)

**Network Infrastructure:**
- Public IP addresses and ranges
- Load balancers
- CDN endpoints
- DNS servers
- VPN gateways
- Firewalls (if externally visible)

**Compute Resources:**
- Web servers
- Application servers
- API servers
- Database servers (if exposed)
- Microservices
- Serverless functions (API Gateway endpoints, etc.)

**Storage:**
- S3 buckets or equivalent
- Database instances
- File servers
- Backup systems
- Content delivery networks

### Application Assets

**Web Applications:**
- Public websites
- Customer portals
- Admin interfaces
- SaaS products

**APIs:**
- REST APIs
- GraphQL endpoints
- SOAP services
- WebSocket services
- Webhooks

**Mobile Applications:**
- iOS apps
- Android apps
- API backends for mobile

**Desktop Applications:**
- Electron apps
- Native applications
- Auto-update servers

### Code and Development Assets

**Source Code Repositories:**
- GitHub, GitLab, Bitbucket repos
- Public vs. private repositories
- Forked repositories
- Gists and code snippets

**Development Infrastructure:**
- Staging environments
- Development environments
- QA/test environments
- Demo instances
- Sandbox systems

**CI/CD Systems:**
- Jenkins servers
- GitLab CI
- GitHub Actions (workflows)
- CircleCI, Travis CI
- Deployment pipelines

**Package Registries:**
- Published npm packages
- Docker images
- Python packages
- Other published artifacts

### Human Assets

**Employees:**
- Engineering staff
- IT and Security teams
- Executives and leadership
- Customer support
- Contractors and temporary staff

**Email Addresses:**
- Corporate email addresses
- Distribution lists
- Support addresses
- Personal emails used for work

**Social Media Accounts:**
- Corporate accounts
- Employee personal accounts discussing work

### Third-Party Assets

**SaaS Services:**
- CRM systems (Salesforce, HubSpot)
- Communication (Slack, Microsoft Teams)
- Productivity (Google Workspace, Office 365)
- Development (GitHub, GitLab, Jira)

**Vendors and Partners:**
- Cloud providers
- Payment processors
- Email service providers
- CDN providers
- Security services

**Integrations:**
- OAuth connections
- API integrations
- SSO providers
- Data sharing agreements

## Criticality Assessment

Not all assets are equal. A corporate blog has different risk than a payment API.

### Criticality Tiers

**Critical:**
Assets whose compromise would cause severe business impact.

Examples:
- Payment processing systems
- Authentication and authorization services
- Customer databases with PII
- Production environments for revenue-generating services
- Core business applications
- Root account access to cloud providers

Impact of compromise:
- Business shutdown or major operational disruption
- Large-scale data breach
- Significant financial loss (direct or liability)
- Regulatory violations with severe penalties
- Irreparable reputation damage

**High:**
Assets important to operations but not immediately critical.

Examples:
- Internal applications
- Employee data systems
- Development environments with production data
- Backup systems
- Administrative interfaces
- Support and ticketing systems

Impact of compromise:
- Operational disruption (not complete shutdown)
- Limited data exposure
- Moderate financial impact
- Productivity loss
- Stepping stone to critical systems

**Medium:**
Assets that support business but aren't operationally critical.

Examples:
- Marketing websites
- Public documentation
- Staging environments (without production data)
- Analytics and monitoring systems
- Legacy applications with limited use

Impact of compromise:
- Minor operational impact
- Reputational impact
- Use as pivot point
- Information disclosure

**Low:**
Assets with minimal business impact.

Examples:
- Archived websites
- Deprecated systems
- Public blogs and content sites
- Redirects and parked domains
- Test systems without sensitive data

Impact of compromise:
- Minimal business impact
- Potential for phishing or social engineering

### Criticality Determination Factors

**Business function:**
- Revenue generation (directly enables sales)
- Revenue support (customer service, billing)
- Operations (internal tools, productivity)
- Information/marketing (low operational impact)

**User base:**
- External customers (high impact)
- Partners or vendors (moderate impact)
- Internal employees (varies by function)
- Public/anonymous (low impact)

**Data sensitivity (covered next):**
Assets handling sensitive data automatically have elevated criticality.

**Availability requirements:**
- 24/7 uptime required (critical)
- Business hours only (high or medium)
- Best effort (low)

**Dependencies:**
- Many systems depend on it (critical)
- Few dependencies (lower criticality)
- No dependencies (standalone, assess independently)

**Compliance requirements:**
- In scope for PCI, HIPAA, SOC2 (increases criticality)
- No compliance requirements (assess based on function)

## Data Classification

What data does the asset handle, store, or process?

### Data Sensitivity Levels

**Restricted:**
Highest sensitivity. Breach would cause severe harm.

Examples:
- Credit card numbers, CVV, full payment data (PCI scope)
- Social Security Numbers, national IDs
- Health records (HIPAA)
- Authentication credentials (passwords, private keys)
- Cryptographic keys and secrets
- Trade secrets and confidential IP

Access: Strictly limited, encrypted at rest and in transit, logged access.

**Confidential:**
Sensitive business or customer data.

Examples:
- Customer PII (names, addresses, emails, phone numbers)
- Financial data (non-payment card)
- Employee records
- Proprietary business data
- Internal communications
- Source code
- Unreleased product information

Access: Need-to-know basis, encrypted in transit, access logging.

**Internal:**
Not public but not highly sensitive.

Examples:
- Internal documentation
- Employee directories
- Organizational charts
- Internal policies
- Non-sensitive business processes
- Aggregate analytics data

Access: Employees and authorized contractors, encrypted in transit.

**Public:**
Intended for public consumption.

Examples:
- Marketing websites
- Public documentation
- Press releases
- Public blog posts
- Open source code

Access: Unrestricted, but integrity protection matters.

### Data Type Inventory

For each asset, document:

**Personal Identifiable Information (PII):**
- Names
- Email addresses
- Physical addresses
- Phone numbers
- Dates of birth
- IP addresses (in some jurisdictions)

**Financial Data:**
- Payment card information
- Bank account numbers
- Transaction records
- Billing information

**Authentication Data:**
- Passwords (even if hashed)
- API keys and tokens
- OAuth tokens
- Session identifiers
- Biometric data

**Health Information:**
- Medical records
- Health insurance information
- Prescription data

**Proprietary Information:**
- Trade secrets
- Source code
- Business strategies
- Customer lists
- Pricing information

### Compliance Mapping

Map data types to compliance requirements:

**PCI-DSS:**
Assets that store, process, or transmit cardholder data.
- Payment processing systems
- Databases with card information
- Systems that transmit card data to payment processors

**HIPAA:**
Protected Health Information (PHI).
- Electronic health records
- Patient portals
- Healthcare provider systems

**GDPR:**
Personal data of EU residents.
- Any system collecting EU resident PII
- Analytics and tracking
- Customer databases

**SOC2:**
Customer data and system security.
- All customer-facing systems
- Data processing systems
- Security controls and logging

**CCPA:**
Personal information of California residents.
- Systems collecting California resident data
- Sale or sharing of personal information

**Industry-specific:**
- FERPA (education records)
- GLBA (financial institution data)
- ITAR (defense and aerospace)

## Dependency Mapping

Assets don't exist in isolation. Map the relationships.

### Types of Dependencies

**Technical dependencies:**
- Application depends on database
- Frontend depends on API
- Service depends on authentication provider
- System depends on DNS
- Workflow depends on third-party API

**Data flow dependencies:**
- User data flows from web app to database
- Orders flow from e-commerce to payment processor
- Logs flow from applications to SIEM
- Backups flow from production to storage

**Network dependencies:**
- Service requires network connectivity
- VPN required for access
- Specific ports and protocols
- Firewall rules enabling communication

**Operational dependencies:**
- Requires specific employee knowledge
- Depends on vendor support
- Requires manual processes
- Depends on specific time-of-day availability

### Dependency Visualization

Create dependency graphs:

```
[Customer] --> [Web App] --> [API Gateway] --> [API Service]
                                   |                 |
                                   v                 v
                              [WAF/CDN]        [Database]
                                                      |
                                                      v
                                                  [Backups]
```

Or tabular format:

```
Asset: A047 (api.example.com)

Upstream Dependencies (this asset needs):
- A023: PostgreSQL database (data storage)
- A089: Redis cache (session storage)
- A112: Auth service (token validation)
- A234: AWS Route53 (DNS resolution)
- A198: AWS ALB (load balancing)

Downstream Dependencies (these need this asset):
- A003: iOS app (API consumer)
- A004: Android app (API consumer)
- A156: Admin dashboard (API consumer)

Third-Party Dependencies:
- Stripe API (payment processing)
- SendGrid API (email notifications)
- Twilio API (SMS notifications)
```

### Single Points of Failure

Identify assets that, if compromised or unavailable, would cascade:

**Critical paths:**
- Authentication service failure blocks everything
- Database failure stops all transactions
- DNS failure makes services unreachable
- Payment processor integration failure stops revenue

Document these explicitly. They become high-priority for both security and availability.

## Network Topology Documentation

Map how assets connect.

### External vs. Internal

**Internet-facing (external attack surface):**
- Web applications
- APIs
- Mail servers
- VPN gateways
- DNS servers

**Internal (accessible after initial compromise):**
- Application servers behind load balancers
- Database servers (not directly exposed)
- Internal file shares
- Admin interfaces on internal networks
- Development systems

**Hybrid:**
- VPN endpoints (external) providing internal access
- Bastion hosts
- Jump boxes

### Network Segmentation

Document network segments if known or observable:

```
DMZ Segment:
- Web servers
- Load balancers
- WAF

Application Segment:
- Application servers
- API servers
- Caching layers

Data Segment:
- Database servers
- Storage systems

Management Segment:
- Admin interfaces
- Monitoring systems
- Jump boxes
```

Even without internal access, you can infer segmentation:
- Web servers in different IP ranges than databases
- Geographic separation (different cloud regions)
- Different cloud accounts or VPCs

### Data Flow Diagrams

Document how data moves:

```
User (Internet)
    |
    v
[Cloudflare CDN/WAF]
    |
    v
[AWS ALB (us-east-1)]
    |
    v
[API Servers (Private Subnet)]
    |
    +---> [PostgreSQL RDS (Private Subnet)]
    +---> [Redis ElastiCache (Private Subnet)]
    +---> [S3 Bucket (us-east-1)]
    +---> [Third-Party APIs (Internet)]
```

This shows:
- Entry points (CDN)
- Trust boundaries (public to private subnet)
- Data destinations (database, cache, storage, third-party)

## Asset Relationships and Context

### Business Context

For each major asset, understand:

**Business purpose:**
What business function does it serve?
- Customer acquisition (marketing site)
- Revenue generation (e-commerce)
- Customer support (help desk)
- Internal operations (HR system)

**User base:**
Who uses it and how?
- External customers (high exposure)
- Partners (moderate exposure)
- Employees (internal exposure)
- Automated systems (M2M)

**Operational windows:**
When must it be available?
- 24/7 (e-commerce, SaaS)
- Business hours (internal tools)
- Scheduled (batch processing)

**Change frequency:**
How often is it updated?
- Continuous deployment (daily/hourly)
- Regular releases (weekly/monthly)
- Stable (quarterly/annually)
- Legacy (rarely/never)

### Technical Context

**Architecture patterns:**
- Monolithic application
- Microservices
- Serverless
- Container-based
- Traditional VM-based

**Hosting models:**
- Self-managed infrastructure
- IaaS (AWS EC2, Azure VMs)
- PaaS (Heroku, Cloud Foundry)
- Fully managed (SaaS backends)

**Integration points:**
- REST APIs
- Message queues
- Event streams
- Database connections
- File transfers

## Organizing the Inventory

### Spreadsheet Structure

**Tab 1: Assets Master List**
Complete inventory with all assets.

**Tab 2: Web Applications**
Filtered view of web apps with web-specific fields.

**Tab 3: APIs**
API inventory with endpoint details.

**Tab 4: Infrastructure**
Servers, IPs, cloud resources.

**Tab 5: Repositories**
Code repositories and packages.

**Tab 6: Third-Party Services**
SaaS and vendor relationships.

**Tab 7: Employees**
Employee list (if relevant for social engineering assessment).

**Tab 8: Dependencies**
Relationship mapping.

**Tab 9: Data Flows**
Data flow documentation.

### Database Approach

For larger engagements, use a proper database:

```sql
CREATE TABLE assets (
    asset_id VARCHAR(10) PRIMARY KEY,
    asset_type VARCHAR(50),
    asset_name VARCHAR(255),
    description TEXT,
    owner VARCHAR(100),
    discovery_method VARCHAR(100),
    discovery_date DATE,
    status VARCHAR(20),
    technology_stack TEXT,
    cloud_provider VARCHAR(50),
    region VARCHAR(50),
    ip_address VARCHAR(50),
    criticality VARCHAR(20),
    data_sensitivity VARCHAR(20),
    business_impact TEXT,
    compliance_scope VARCHAR(100),
    notes TEXT
);

CREATE TABLE dependencies (
    dependency_id INT PRIMARY KEY AUTO_INCREMENT,
    asset_id VARCHAR(10),
    depends_on VARCHAR(10),
    dependency_type VARCHAR(50),
    description TEXT,
    FOREIGN KEY (asset_id) REFERENCES assets(asset_id),
    FOREIGN KEY (depends_on) REFERENCES assets(asset_id)
);

CREATE TABLE data_flows (
    flow_id INT PRIMARY KEY AUTO_INCREMENT,
    source_asset VARCHAR(10),
    destination_asset VARCHAR(10),
    data_type VARCHAR(100),
    protocol VARCHAR(50),
    encryption BOOLEAN,
    FOREIGN KEY (source_asset) REFERENCES assets(asset_id),
    FOREIGN KEY (destination_asset) REFERENCES assets(asset_id)
);
```

Query for specific analysis:
```sql
-- Find all critical assets
SELECT * FROM assets WHERE criticality = 'Critical';

-- Find all assets handling restricted data
SELECT * FROM assets WHERE data_sensitivity = 'Restricted';

-- Find all dependencies for a specific asset
SELECT a.asset_name, d.depends_on, a2.asset_name as depends_on_name
FROM assets a
JOIN dependencies d ON a.asset_id = d.asset_id
JOIN assets a2 ON d.depends_on = a2.asset_id
WHERE a.asset_id = 'A047';

-- Find single points of failure (assets with many dependents)
SELECT depends_on, COUNT(*) as dependent_count
FROM dependencies
GROUP BY depends_on
ORDER BY dependent_count DESC;
```

## Asset Verification

Don't just catalog - verify your understanding is correct.

### Verification Methods

**Technology validation:**
- Compare fingerprinting results from multiple tools
- Check HTTP headers, JavaScript sources, error messages
- Verify versions against known releases

**Ownership validation:**
- Confirm domains registered to correct organization
- Verify IP ranges via WHOIS and ASN lookups
- Check cloud resource account ownership (if accessible)

**Status validation:**
- Verify assets are actually active (not stale DNS records)
- Confirm services respond as expected
- Check SSL certificate validity and ownership

**Dependency validation:**
- Trace network connections (if possible)
- Verify API integrations through observation
- Confirm data flows through documentation or testing

### Handling Ambiguity

**Unknown ownership:**
Some assets may not clearly belong to the organization:
- Domains registered to employees personally
- Cloud resources in unclear accounts
- Third-party managed infrastructure

Document as "Verify with client" and flag for clarification.

**Uncertain status:**
Is that staging environment still in use?
- Check for recent SSL certificate issuance
- Look for recent DNS changes
- Check repository commit history
- Flag for client confirmation

**Unclear data handling:**
You may not know what data an asset handles externally:
- Make educated guesses based on purpose
- Mark as "To be confirmed"
- Ask client for clarification
- Default to more sensitive classification if uncertain

## Common Challenges

**Scale:**
Large organizations may have thousands of assets.
Solution: Start with internet-facing assets, prioritize based on criticality, use automation for bulk categorization.

**Dynamic infrastructure:**
Cloud environments change constantly.
Solution: Timestamp all discoveries, note ephemeral vs. persistent resources, plan for updates.

**Incomplete information:**
You won't know everything from external reconnaissance.
Solution: Document what you know, mark gaps, provide assumptions, plan client interviews.

**Classification disagreement:**
You might classify something as critical while the organization considers it low priority.
Solution: Document your reasoning, provide framework for classification, allow client input, include both perspectives in final report.

## Deliverables from This Phase

**Asset inventory spreadsheet/database:**
Complete, structured list of all discovered assets.

**Asset classification report:**
Summary of asset counts by type, criticality, and sensitivity.

**Dependency maps:**
Visual or tabular representation of key dependencies.

**Data flow diagrams:**
How sensitive data moves through systems.

**Verification questions:**
List of items requiring client clarification.

**Summary statistics:**
- Total assets discovered
- Breakdown by type
- Critical/High/Medium/Low counts
- Compliance scope coverage
- Third-party service count

## Next Steps

With the asset inventory complete and classified, you're ready for threat identification. Chapter 4 covers identifying threat actors, understanding motivations, and mapping specific threats to the asset inventory.

The inventory tells you what you have. Threat identification tells you who wants it and why.