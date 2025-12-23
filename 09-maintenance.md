# Chapter 9: Maintenance and Continuous Monitoring

## Overview

Threat models decay. New assets appear. Old ones disappear. Threats evolve. Controls degrade. Last quarter's assessment is already outdated.

This chapter covers keeping the threat model current. Update triggers, continuous monitoring, metrics, and version control. A threat model is a living document, not a PDF gathering dust.

## Why Threat Models Become Outdated

### Environmental Changes

**New assets:**
```
Examples:
- New product launch (new domain, new API)
- Acquisition (inherited infrastructure)
- Cloud migration (changed architecture)
- SaaS adoption (new third-party risk)
- Shadow IT (unauthorized systems)

Impact:
- Attack surface expansion
- New threats
- Different risk profile
- Gaps in controls
```

**Removed assets:**
```
Examples:
- Decommissioned systems (but DNS still points there)
- Migrated services (old ones not shut down)
- Acquired company integration (duplicates)

Impact:
- Orphaned systems (security gaps)
- Subdomain takeover risks
- Wasted security resources
- Confused asset inventory
```

**Changed assets:**
```
Examples:
- Software upgrades (new vulnerabilities)
- Configuration changes (weakened security)
- Technology stack changes (different risks)
- Moved to different infrastructure (cloud region, provider)

Impact:
- Risk profile changes
- Control effectiveness changes
- Different compliance requirements
```

### Threat Landscape Evolution

**Emerging threats:**
```
Examples:
- New attack techniques (zero-day exploits)
- New threat actors (industry-specific APT)
- New malware families (ransomware variants)
- New exploitation methods (supply chain)

Impact:
- Previously low risks become high
- New attack vectors emerge
- Existing controls become ineffective
```

**Resolved threats:**
```
Examples:
- Patched vulnerabilities
- Disrupted threat actor infrastructure
- Law enforcement takedowns
- Outdated attack methods

Impact:
- Risk reduction (update threat model)
- Resource reallocation
```

### Control Changes

**Implemented controls:**
```
Impact:
- Reduced residual risk
- New attack paths may emerge
- Maintenance requirements
- New dependencies
```

**Failed controls:**
```
Examples:
- Misconfigured after change
- Disabled for troubleshooting (never re-enabled)
- Bypassed for convenience
- Software bugs
- Service degradation

Impact:
- Risk increases
- False sense of security
- Gaps in detection
```

## Update Triggers

### Scheduled Reviews

**Quarterly reviews (minimum):**
```
Quarterly Checklist:

Asset Changes:
- New assets discovered (monitoring alerts)
- Decommissioned assets (verify actually gone)
- Changed assets (technology, location, purpose)
- Update asset inventory
- Reclassify if needed

Threat Updates:
- Review threat intelligence
- Industry incidents (lessons learned)
- New threat actors or campaigns
- Update threat profiles
- Adjust likelihood ratings

Risk Reassessment:
- Review risk register
- Update risk scores based on changes
- Verify control effectiveness
- Adjust priorities

Metric Review:
- Track risk trends
- Measure control effectiveness
- Review remediation progress
- Report to stakeholders
```

**Annual comprehensive review:**
```
Annual Assessment:

Full Discovery:
- Complete reconnaissance refresh
- Enumerate all assets (not just changes)
- Deep dive on critical systems
- Third-party audit (consider external)

Methodology Update:
- Review and update threat modeling approach
- Incorporate lessons learned
- Update templates and checklists
- Tool evaluation

Strategic Review:
- Align with business strategy
- Update risk appetite
- Adjust security roadmap
- Budget planning for next year

Compliance:
- SOC2 annual audit
- ISO27001 certification review
- Regulatory requirement updates
- Industry standard changes
```

### Event-Triggered Reviews

**Major changes:**
```
Triggers for Immediate Threat Model Update:

New Product Launch:
- Full threat model for new asset
- Integration with existing model
- Update overall risk posture

Acquisition or Merger:
- Assess acquired company infrastructure
- Integration risks
- Inherited technical debt
- Cultural and process gaps

Infrastructure Changes:
- Cloud migration
- Data center changes
- Network redesign
- Technology stack changes

Major Software Deployments:
- New customer-facing applications
- Core system replacements
- Critical infrastructure updates

Organizational Changes:
- Leadership changes (security priorities shift)
- Team restructuring (ownership changes)
- Office locations (physical security)
- Workforce changes (insider risk)
```

**Security incidents:**
```
Post-Incident Update:

After any security incident:
1. Incident analysis
   - What happened
   - How they got in
   - What was compromised
   - Why controls failed

2. Threat model update
   - Add incident as threat scenario
   - Update likelihood (proven attack path)
   - Identify similar exposures
   - Update controls

3. Lessons learned
   - What was missed in threat model
   - Why wasn't this identified
   - How to prevent similar gaps
   - Process improvements

4. Risk reassessment
   - Adjust risk ratings
   - Reprioritize mitigations
   - Update risk register
```

**Threat intelligence:**
```
Intelligence-Driven Updates:

New vulnerability disclosure:
- Check if organization is affected
- Update vulnerability assessment
- Adjust risk ratings
- Prioritize patching

Active exploitation campaigns:
- Industry-specific attacks
- Threat actor activity
- Attack technique trends
- Adjust likelihood ratings

Breach notifications:
- Similar organizations compromised
- Attack methods used
- Lessons applicable
- Update scenarios
```

### Continuous Monitoring Triggers

**Automated alerts:**
```
Set up monitoring for:

New Assets:
- Certificate transparency monitoring (new subdomains)
- DNS changes (new records)
- IP allocation (new ranges)
- Cloud resource creation (AWS Config, Azure Activity)
- Code repository creation (GitHub, GitLab)

Alert trigger: New asset requires threat assessment

Configuration Changes:
- S3 bucket permission changes
- Firewall rule modifications
- IAM policy changes
- Network security group updates

Alert trigger: Review for security impact

Anomalous Activity:
- Unusual access patterns
- Failed authentication spikes
- Large data transfers
- Off-hours access
- Geographic anomalies

Alert trigger: Potential threat realization

Control Failures:
- Security tool errors
- Monitoring gaps
- Backup failures
- Certificate expirations

Alert trigger: Risk increase (control degradation)
```

## Continuous Asset Discovery

### Passive Monitoring

**Certificate transparency monitoring:**
```
Tools:
- Certstream (real-time CT log monitoring)
- crt.sh API
- Facebook CT Monitoring

Implementation:
# Monitor for new certificates
curl -s "https://crt.sh/?q=%25.example.com&output=json&exclude=expired" \
  | jq -r '.[].name_value' \
  | sort -u > current-certs.txt

# Diff against previous
diff previous-certs.txt current-certs.txt

# Alert on new subdomains
# Integrate with asset inventory

Frequency: Daily or real-time
```

**DNS monitoring:**
```
Monitor DNS changes:
- New A/AAAA records
- CNAME changes
- MX record updates
- TXT record modifications

Tools:
- SecurityTrails API
- DNSdumpster
- Custom scripts with dig

Alert on:
- New subdomains not in inventory
- Changed IP addresses
- Removed records (potential takeover)
```

**Cloud resource monitoring:**
```
AWS:
- AWS Config (track resource changes)
- CloudTrail (API activity)
- Trusted Advisor (security checks)

Azure:
- Azure Activity Log
- Azure Security Center
- Azure Resource Graph

GCP:
- Cloud Asset Inventory
- Cloud Logging
- Security Command Center

Alert on:
- New EC2 instances, S3 buckets, databases
- Permission changes
- Public exposure
- Non-compliant configurations
```

### Active Scanning

**Scheduled reconnaissance:**
```
Weekly Scans:
- Subdomain enumeration (automated)
- Port scanning (external IPs)
- SSL/TLS monitoring (expiration, config)
- Web application fingerprinting

Monthly Scans:
- Vulnerability scanning (authenticated + unauthenticated)
- Cloud misconfiguration checks
- Third-party service enumeration
- Dark web monitoring (credential leaks)

Compare against baseline:
- New assets
- Changed configurations
- New vulnerabilities
- Removed assets

Update asset inventory automatically where possible
```

**Automated tooling:**
```
Continuous Reconnaissance Pipeline:

1. Discovery (daily):
   - subfinder -d example.com
   - amass enum -passive -d example.com
   - Output to database

2. Probing (daily):
   - httpx against discovered domains
   - Screenshot new assets
   - Technology fingerprinting

3. Vulnerability Scanning (weekly):
   - nuclei against all web assets
   - nmap service scans
   - Cloud security posture (prowler, ScoutSuite)

4. Comparison (automated):
   - Diff against previous runs
   - Identify new/changed/removed
   - Generate alerts

5. Integration (automated):
   - Update asset inventory
   - Create tickets for review
   - Risk assessment for new assets
```

## Threat Intelligence Integration

### Intelligence Sources

**Free sources:**
```
Government/Public:
- CISA alerts (https://www.cisa.gov/cybersecurity-advisories)
- US-CERT bulletins
- NIST National Vulnerability Database
- MITRE ATT&CK updates

Open Source:
- Twitter/X security community
- Reddit (r/netsec, r/cybersecurity)
- Security blogs (Krebs, Schneier, etc.)
- Vendor threat reports (CrowdStrike, Mandiant)

Industry:
- ISACs (Information Sharing and Analysis Centers)
- Industry-specific alerts
- Peer networks
- Conference presentations

Implementation:
- RSS feeds aggregated
- Daily review process
- Filter for relevance
- Weekly threat briefing
```

**Commercial sources:**
```
Threat Intelligence Platforms:
- Recorded Future
- ThreatConnect
- Anomali
- Digital Shadows

Benefits:
- Automated indicator matching
- Threat actor tracking
- Vulnerability prioritization
- Dark web monitoring

Integration:
- API integration with SIEM
- Automated threat hunting
- Risk scoring updates
- Alert enrichment
```

### Intelligence-Driven Updates

**Process:**
```
1. Intelligence Collection
   - Daily feed review
   - Automated aggregation
   - Filter for relevance

2. Analysis
   - Applicable to organization?
   - Affects existing assets?
   - Changes likelihood/impact?
   - New threat scenarios?

3. Threat Model Update
   - Add new threat scenarios
   - Update existing threat profiles
   - Adjust likelihood ratings
   - Document intelligence source

4. Risk Reassessment
   - Recalculate affected risks
   - Reprioritize if needed
   - Update risk register

5. Action
   - Notify stakeholders
   - Implement mitigations
   - Update monitoring
   - Hunt for indicators

6. Documentation
   - Update threat model
   - Version control
   - Change log
```

**Example:**
```
Intelligence: New ransomware variant targeting industry

1. Collection:
   - CISA alert: New ransomware "ExampleLocker"
   - Targeting technology companies
   - Initial access via phishing
   - Exploits unpatched VPN vulnerabilities

2. Analysis:
   - Applicable: Yes (technology company)
   - VPN in use: Yes (Check patch status)
   - Phishing controls: Partial
   - Backup strategy: Exists but not tested recently

3. Update Threat Model:
   - Add "ExampleLocker Ransomware" scenario
   - Update ransomware threat profile
   - Likelihood: HIGH (actively targeting industry)
   - Impact: VERY HIGH (existing assessment)

4. Risk Assessment:
   - Previous risk: MEDIUM (lower likelihood)
   - Updated risk: CRITICAL (higher likelihood)
   - Moved to top priority

5. Action:
   - Verify VPN patch status (URGENT)
   - Test backup restoration (URGENT)
   - Enhanced phishing training
   - Threat hunting for IoCs
   - Update EDR signatures

6. Documentation:
   - Added TM-2025-047: ExampleLocker Scenario
   - Updated risk register (R-008 → CRITICAL)
   - Remediation plan updated
   - Stakeholders notified
```

## Metrics and KPIs

### Risk Metrics

**Track over time:**
```
Overall Risk Posture:
- Total risk exposure (ALE)
- Risk by category (Critical/High/Medium/Low)
- Trend (improving/stable/degrading)

Example Dashboard:
Current Quarter:
- Critical risks: 3 (down from 7)
- High risks: 18 (down from 25)
- Medium risks: 45 (up from 38)
- Low risks: 34 (stable)
- Total ALE: $8.2M (down from $12.3M)

Trend: IMPROVING (33% risk reduction)
```

**Risk velocity:**
```
Risk Creation vs Resolution:

Q4 2025:
- New risks identified: 15
- Risks mitigated/closed: 22
- Risks accepted: 3
- Net change: -7 (good)

Risk Age:
- Average time to closure: 45 days
- Overdue (>90 days): 8 risks
- Blocked risks: 2

Goal: Net negative risk creation (close more than create)
```

### Asset Metrics

**Asset discovery:**
```
Track asset inventory growth:

Asset Count by Quarter:
Q1: 189 assets
Q2: 217 assets (+28, new product launch)
Q3: 203 assets (-14, decommissioned legacy)
Q4: 198 assets (-5, consolidation)

Discovery rate:
- Automated: 85%
- Manual: 15%

Goal: >90% automated discovery

Asset classification:
- Critical: 12 (6%)
- High: 45 (23%)
- Medium: 98 (49%)
- Low: 43 (22%)

Goal: <10% critical assets
```

### Control Effectiveness

**Measure control performance:**
```
Security Controls Scorecard:

Authentication Controls:
- MFA enrollment: 87% (target: 100%)
- Password policy compliance: 95%
- Failed login rate: 0.3% (acceptable)
- Account lockouts: 12/month (monitor)

Network Controls:
- Firewall rule compliance: 98%
- Segmentation effectiveness: 65% (needs improvement)
- IDS/IPS detection rate: 78% (baseline)

Vulnerability Management:
- Critical vulns (mean time to patch): 5 days (target: 7)
- High vulns (MTTP): 18 days (target: 30)
- Vulnerability backlog: 47 (down from 89)

Backup and Recovery:
- Backup success rate: 99.2%
- Recovery test frequency: Monthly (meets target)
- RTO compliance: 95% (target: >90%)
```

### Remediation Metrics

**Track progress:**
```
Remediation Tracking:

By Priority:
- Critical (0-30 days): 2 of 3 complete (67%)
- High (1-3 months): 15 of 23 in progress (65%)
- Medium (3-6 months): 38 of 67 planned (57%)

Overall:
- On track: 72%
- Behind schedule: 21%
- Blocked: 7%

Blockers:
- Budget constraints: 3 items
- Resource availability: 4 items
- Technical complexity: 2 items

Actions:
- Escalate budget items to CFO
- Reallocate resources for critical items
- Engage consultants for complex items
```

## Version Control and Change Management

### Threat Model Versioning

**Version scheme:**
```
Version Format: MAJOR.MINOR.PATCH

MAJOR: Significant methodology change or full reassessment
MINOR: Quarterly updates or substantial new sections
PATCH: Minor corrections or small updates

Example:
v1.0.0 - Initial threat model (2025-01-15)
v1.1.0 - Q2 quarterly update (2025-04-15)
v1.1.1 - Correction to risk scores (2025-05-02)
v1.2.0 - Q3 quarterly update (2025-07-15)
v2.0.0 - Annual comprehensive reassessment (2026-01-15)
```

**Change log:**
```
CHANGELOG - Organizational Threat Model

v1.2.0 (2025-07-15)
Added:
- 12 new assets discovered (new marketing campaigns)
- Threat scenario: AI-powered phishing (TM-2025-089)
- Risk assessment for new mobile app launch

Changed:
- Updated ransomware likelihood (HIGH → VERY HIGH) based on industry attacks
- Revised cloud infrastructure diagram (AWS region expansion)
- Risk R-023 severity (MEDIUM → HIGH) due to control degradation

Removed:
- 5 decommissioned legacy systems
- Accepted risk R-015 (control implemented)

Fixed:
- Corrected asset owner for A-047 (was incorrect)
- Updated compliance status (SOC2 completed)

v1.1.1 (2025-05-02)
Fixed:
- Risk calculation error for R-034 (was MEDIUM, should be HIGH)
- Typo in mitigation timeline for TM-2025-045

v1.1.0 (2025-04-15)
Added:
- Quarterly update: 8 new assets
- Threat intelligence: New APT group targeting industry
- Vulnerability: CVE-2025-XXXX assessment

Changed:
- Updated threat actor profiles (3 actors)
- Revised risk matrix based on implemented controls
- Adjusted timeline for remediation roadmap

Removed:
- Closed risks: R-001, R-008, R-015 (mitigated)
```

### Document Storage

**Version control system:**
```
Use Git for version control:

Repository structure:
threat-model-example-corp/
├── README.md
├── CHANGELOG.md
├── current/
│   ├── executive-report.pdf
│   ├── technical-report.md
│   ├── presentation.pptx
│   ├── asset-inventory.xlsx
│   └── risk-register.xlsx
├── archive/
│   ├── v1.0.0/
│   ├── v1.1.0/
│   └── v1.2.0/
├── templates/
│   ├── finding-template.md
│   ├── risk-template.md
│   └── report-template.md
└── intelligence/
    ├── 2025-Q1/
    ├── 2025-Q2/
    └── 2025-Q3/

Benefits:
- Full history
- Change tracking
- Collaboration
- Backup
- Rollback capability
```

## Stakeholder Communication

### Regular Reporting

**Monthly security briefing:**
```
To: CISO, CTO, Executive Team
Format: 1-page summary + risk dashboard

Topics:
- Risk posture changes
- New critical findings
- Remediation progress
- Upcoming priorities
- Budget/resource needs

Example:
---
SECURITY POSTURE UPDATE - October 2025

Overall Status: STABLE (improved from last month)

Risk Summary:
- Critical: 2 (down from 3)
- High: 18 (stable)
- Total ALE: $7.8M (down from $8.2M)

Key Activities:
- Closed R-045: MFA implementation complete
- New finding: F-2025-089 (HIGH severity, ransomware exposure)
- Remediation: 85% on-track

Upcoming:
- Q4 threat model update
- SOC2 audit preparation
- Vendor risk assessments

Needs:
- Budget approval: EDR expansion ($50k)
- Resource allocation: 2 FTE for network segmentation
---
```

**Quarterly business review:**
```
To: Board, Executive Team, Business Leaders
Format: Presentation + detailed report

Topics:
- Quarterly threat model update
- Strategic risk overview
- Business impact analysis
- Security roadmap progress
- Investment ROI
- Industry benchmarking

Metrics:
- Risk reduction achieved
- Incidents prevented
- Compliance status
- Budget utilization
- Maturity progression
```

### Incident Communication

**Security incident reporting:**
```
When threat is realized:

Immediate (within hours):
- Incident notification
- Impact assessment
- Containment status
- Immediate actions

Post-incident (within days):
- Incident report
- Root cause analysis
- Threat model update
- Lessons learned
- Remediation plan

Follow-up (within weeks):
- Threat model revised
- Similar exposures identified
- Controls updated
- Training/awareness
```

## Continuous Improvement

### Process Refinement

**Regular retrospectives:**
```
After each quarterly update:

What worked well:
- Automated asset discovery caught new systems
- Threat intelligence integration caught emerging threat
- Metrics showed progress

What needs improvement:
- Manual effort still too high for some tasks
- Tool integration gaps
- Reporting process time-consuming

Actions:
- Invest in automation (specific tools)
- Improve tool integration (APIs)
- Template improvements (reduce manual work)
```

### Automation Opportunities

**Automate repetitive tasks:**
```
Candidates for automation:

Asset Discovery:
- Certificate transparency monitoring → Automated
- Subdomain enumeration → Scheduled daily
- Cloud resource inventory → API-driven
- Asset database updates → Auto-populated

Risk Scoring:
- CVSS calculation → Automated
- Likelihood updates based on threat intel → Automated
- Risk matrix generation → Script-based

Reporting:
- Metric dashboards → Auto-updated
- Risk trend charts → Generated from database
- Status reports → Template-based with data injection

Monitoring:
- Configuration drift detection → Automated alerts
- Vulnerability scanning → Continuous
- Threat hunting → Partially automated (IOC matching)
```

## Long-Term Sustainability

### Resource Planning

**Dedicated ownership:**
```
Threat Modeling Program:

Owner: CISO or Security Architect
Time commitment: 20-40% FTE (depending on org size)

Supporting roles:
- Security analysts: Reconnaissance and monitoring
- Risk analysts: Risk scoring and tracking
- IT teams: Asset inventory accuracy
- Business analysts: Impact assessment

Budget:
- Tools: $50k-$200k/year (depending on scale)
- Training: $10k-$20k/year
- External assessments: $50k-$150k/year (periodic)
- Staff time: Calculate based on FTE allocation
```

### Integration with Security Program

**Align with other security activities:**
```
Threat Model Integration:

Vulnerability Management:
- Threat model identifies assets
- Vuln management prioritizes based on asset criticality
- Feed findings back to threat model

Incident Response:
- Threat model provides context
- IR validates threat scenarios
- Incidents update threat model

Security Architecture:
- Threat model informs design decisions
- Architecture reviews update threat model
- New projects trigger threat assessment

GRC (Governance, Risk, Compliance):
- Shared risk register
- Compliance mapping
- Policy alignment
- Audit evidence
```

## Conclusion

Threat modeling is continuous work. The initial assessment is just the beginning.

Maintain the model:
- Schedule regular reviews (quarterly minimum)
- Monitor continuously (automate what you can)
- Respond to triggers (incidents, intel, changes)
- Track metrics (measure improvement)
- Communicate (keep stakeholders informed)
- Improve processes (retrospectives and automation)

A threat model is only valuable if it's current. An outdated threat model is worse than none - it provides false confidence.

Keep it alive. Keep it accurate. Keep it actionable.