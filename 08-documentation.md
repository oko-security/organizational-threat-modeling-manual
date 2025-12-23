# Chapter 8: Documentation and Reporting

## Overview

All the reconnaissance, analysis, and risk assessment is worthless if you can't communicate it effectively.

This chapter covers packaging your findings into deliverables. Different audiences need different information. Executives want business impact. Engineers want technical details. Compliance wants evidence of controls.

Write for your audience. Be clear. Be actionable. Get to the point.

## Report Structure

### Multi-Document Approach

One report doesn't fit all audiences.

**Executive Report:**
- 5-10 pages
- Business language
- High-level findings
- Risk priorities
- Cost and timeline for fixes

**Technical Report:**
- 50-200+ pages
- Technical details
- Specific vulnerabilities
- Remediation instructions
- Evidence and screenshots

**Presentation:**
- 15-30 slides
- Visual storytelling
- Key findings
- Recommendations
- Q&A preparation

**Supporting Documents:**
- Asset inventory spreadsheet
- Risk register
- Threat scenarios
- Attack surface maps
- Remediation roadmap

## Executive Report

### Executive Summary

First page. Sometimes the only page executives read.

**Structure:**
```
1. Purpose and scope (2-3 sentences)
2. Key findings (3-5 bullets)
3. Critical risks (top 3-5)
4. Business impact (dollar figures)
5. Recommendations (priority actions)
6. Investment required (budget)
```

**Example:**
```
EXECUTIVE SUMMARY

Purpose:
This threat model assessed Example Corp's entire digital footprint to identify security risks that could impact business operations, customer trust, and regulatory compliance.

Key Findings:
- Discovered 247 internet-facing assets, including 4 with critical security gaps
- Identified 127 distinct threats, with 4 requiring immediate action
- Found exposed customer database backup containing 250,000 records
- Weak authentication controls enable account takeover attacks
- Estimated annual loss exposure: $12.3M without mitigation

Critical Risks:
1. Public S3 bucket exposing customer backups ($10M+ potential impact)
2. Exposed database port on legacy system ($8M+ potential impact)
3. Credential stuffing attacks on customer portal ($2.5M+ potential impact)
4. Unmitigated ransomware risk ($4.8M annual loss expectancy)

Business Impact:
Current security gaps expose Example Corp to:
- Regulatory fines: Up to $15M (GDPR, PCI violations)
- Customer data breach: 250,000+ records at risk
- Operational disruption: Revenue loss of $410k per 72-hour outage
- Reputation damage: Estimated 30% customer churn ($375M impact)

Recommendations:
Immediate (0-30 days):
- Fix S3 bucket permissions (0 cost, 1 hour)
- Close exposed database port (0 cost, 1 hour)
- Deploy rate limiting on authentication (1 week, $10k)

Short-term (1-3 months):
- Implement MFA for all users (4 weeks, $15k)
- Deploy endpoint detection and response (6 weeks, $120k)
- Enhance backup and recovery ($50k setup, $30k/year)

Long-term (3-12 months):
- Security awareness program (ongoing, $65k/year)
- Network segmentation ($200k, 12 weeks)
- Zero trust architecture (phased, $500k+)

Investment Required:
- Immediate fixes: $10k
- Critical risk mitigation (90 days): $185k
- Annual security program: $95k/year
- Total first-year investment: $280k

ROI: Estimated risk reduction of $10M+ annually

Next Steps:
1. Review and approve critical fixes (immediate)
2. Allocate budget for 90-day roadmap
3. Schedule monthly risk reviews
4. Assign remediation owners
```

### Business Impact Section

Translate technical findings to business consequences.

**Framework:**
```
Technical Finding -> Business Impact

BAD:
"The API has an IDOR vulnerability"

GOOD:
"A security gap in the customer API allows unauthorized access to any customer's personal information, order history, and payment tokens. This could expose 250,000 customer records, violating GDPR and PCI compliance, resulting in fines up to $15M plus lawsuits, customer churn, and reputation damage."
```

**Impact categories:**
```
Financial:
- Direct costs (incident response, legal, fines)
- Revenue loss (downtime, customer churn)
- Recovery costs (rebuilding, improvements)

Operational:
- Service disruption
- Productivity loss
- Resource diversion

Legal/Regulatory:
- Compliance violations
- Fines and penalties
- Lawsuits and settlements

Reputation:
- Customer trust loss
- Media coverage
- Brand damage
- Competitive disadvantage

Strategic:
- Loss of competitive advantage
- IP theft
- Market position impact
```

### Risk Summary Dashboard

Visual risk overview.

```
RISK DASHBOARD

Overall Security Posture: MODERATE
Trend: IMPROVING (from prior assessment)

Risk Distribution:
[Critical] ████ 4 risks (3%)
[High]     ███████████████████ 23 risks (18%)
[Medium]   █████████████████████████████████████ 67 risks (53%)
[Low]      █████████████████ 33 risks (26%)

Top 5 Risks by Annual Loss Expectancy:
1. Ransomware                        $4.8M/year
2. Customer data breach (SQLi)       $2.3M/year
3. Account takeover (credential stuffing) $2.5M/year
4. DDoS against revenue systems      $1.2M/year
5. Insider data exfiltration         $900k/year

Total Risk Exposure: $12.3M/year
Post-Mitigation Exposure: $1.8M/year (with recommended controls)
Risk Reduction: 85%

Assets by Criticality:
Critical: 12 assets
High: 47 assets
Medium: 134 assets
Low: 54 assets

Compliance Status:
PCI-DSS: 3 violations (CRITICAL)
GDPR: 8 gaps (HIGH)
SOC2: 12 gaps (MEDIUM)
```

### Recommendations Section

Actionable, prioritized, with timelines.

```
CRITICAL ACTIONS (0-30 days)

1. Fix Public S3 Bucket Permissions
   Risk: CRITICAL ($10M+ potential impact)
   Effort: 1 hour
   Cost: $0
   Owner: DevOps Lead
   Impact: Immediately protects 250k customer records

   Steps:
   - Audit all S3 buckets for public access
   - Remove public read permissions from example-com-backups
   - Implement bucket policies requiring authentication
   - Enable CloudTrail logging for bucket access
   - Set up alerts for permission changes

2. Close Exposed Database Port
   Risk: CRITICAL ($8M+ potential impact)
   Effort: 1 hour
   Cost: $0
   Owner: Infrastructure Lead
   Impact: Eliminates direct database access from internet

   Steps:
   - Modify security group for legacy.example.com
   - Remove inbound rule for port 3306 from 0.0.0.0/0
   - Add rule allowing only application servers
   - Verify application connectivity post-change
   - Monitor for failed connection attempts

3. Deploy Authentication Rate Limiting
   Risk: CRITICAL ($2.5M+ potential impact)
   Effort: 1 week
   Cost: $10k (implementation)
   Owner: Engineering Lead
   Impact: Blocks credential stuffing attacks

   Steps:
   - Implement rate limiting at load balancer (100 req/hour per IP)
   - Add per-account rate limiting (10 failed attempts/hour)
   - Deploy CAPTCHA after 3 failed attempts
   - Monitor for false positives
   - Adjust thresholds based on data

HIGH PRIORITY ACTIONS (1-3 months)

4. Enforce Multi-Factor Authentication
   Timeline: 4 weeks
   Cost: $15k (already licensed, just implementation)
   Impact: Reduces account takeover risk by 70%

5. Deploy Endpoint Detection and Response
   Timeline: 6 weeks
   Cost: $120k (year 1), $80k/year ongoing
   Impact: Detects ransomware and malware early

6. Implement Backup and Recovery Testing
   Timeline: Ongoing
   Cost: $50k setup, $30k/year
   Impact: Reduces ransomware impact from $4.8M to $600k

[Continue with MEDIUM and LOW priority actions]
```

## Technical Report

### Report Organization

```
TABLE OF CONTENTS

1. Executive Summary (1 page)
2. Methodology (5-10 pages)
   2.1 Scope and Boundaries
   2.2 Tools and Techniques
   2.3 Timeline
   2.4 Limitations
3. Asset Inventory (10-20 pages)
   3.1 Discovered Assets
   3.2 Asset Classification
   3.3 Technology Stack
   3.4 Network Topology
4. Threat Analysis (20-40 pages)
   4.1 Threat Actors
   4.2 Attack Vectors
   4.3 Threat Scenarios
   4.4 Attack Surface Analysis
5. Vulnerability Assessment (30-60 pages)
   5.1 External Vulnerabilities
   5.2 Internal Vulnerabilities
   5.3 Process Vulnerabilities
   5.4 Vulnerability Details (per finding)
6. Risk Assessment (10-20 pages)
   6.1 Risk Methodology
   6.2 Risk Matrix
   6.3 Risk Register
   6.4 Quantitative Analysis
7. Recommendations (20-40 pages)
   7.1 Immediate Actions
   7.2 Short-term Roadmap
   7.3 Long-term Strategy
   7.4 Detailed Remediation Steps
8. Appendices
   A. Asset Inventory Spreadsheet
   B. Risk Register
   C. Tool Output
   D. References
```

### Findings Format

**Consistent structure for each finding:**

```
FINDING: F-001 - Public S3 Bucket Exposing Customer Backups

Severity: CRITICAL
CVSS: N/A (Configuration issue)
Category: Information Disclosure
Asset: example-com-backups.s3.amazonaws.com
Discovered: 2025-12-20

Description:
An Amazon S3 bucket containing database backups is publicly accessible, allowing anyone on the internet to download customer data without authentication. The bucket contains daily PostgreSQL dumps from the production customer database, including full customer records, PII, and payment tokens.

Evidence:
The bucket can be accessed anonymously:

$ aws s3 ls s3://example-com-backups --no-sign-request
2025-12-19 backup-20251219.sql.gz
2025-12-18 backup-20251218.sql.gz
2025-12-17 backup-20251217.sql.gz
[...]

Bucket permissions allow public read:
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-com-backups/*"
  }]
}

Downloaded sample backup and verified contents:
- 250,000 customer records
- Full names, email addresses, phone numbers, addresses
- Order history and transaction data
- Payment tokens (Stripe tokenized, not full card numbers)
- Dates of birth, IP addresses, user agents

Impact:
CRITICAL - Very High Impact

Data Exposure:
- 250,000 customer PII records
- GDPR and CCPA violations (EU and CA residents included)
- Identity theft risk
- Phishing and social engineering targeting

Financial Impact:
- GDPR fines: Up to €20M or 4% global revenue
- CCPA fines: $7,500 per intentional violation × records
- Notification costs: $5M (250k × $20 per notification)
- Legal fees and settlements: $5M+
- Reputation damage: 30% customer churn = $375M
Total: $385M+ exposure

Compliance:
- GDPR Article 32 violation (inadequate security)
- PCI-DSS violation (if any payment data exposed)
- SOC2 failure (access controls)
- Mandatory breach notification required

Likelihood:
HIGH - Data is currently exposed and discoverable through:
- Bucket enumeration tools (publicly available)
- Google dorking for bucket names
- Accidental discovery
- Targeted reconnaissance

Risk:
HIGH Likelihood × CRITICAL Impact = CRITICAL Risk

Recommendation:
IMMEDIATE ACTION REQUIRED (within 24 hours)

1. Remove public access immediately:
   - Remove bucket policy allowing public read
   - Enable "Block all public access" setting
   - Verify bucket is no longer accessible anonymously

2. Rotate credentials and tokens:
   - All API tokens in backups should be rotated
   - Notify customers to change passwords (if password hashes exposed)
   - Invalidate sessions

3. Implement secure backup process:
   - Use private bucket with restricted IAM policies
   - Encrypt backups with KMS (at-rest encryption)
   - Require MFA for bucket access
   - Enable versioning and object lock

4. Audit other S3 buckets:
   - Review all buckets for public access
   - Implement automated checks (AWS Config rules)
   - Set up alerts for public bucket creation

5. Determine exposure timeline:
   - Review CloudTrail logs for bucket access
   - Identify if anyone downloaded backups
   - Determine notification requirements

6. Prepare breach notification:
   - If evidence of access, notify affected individuals
   - Notify regulators per GDPR/CCPA requirements
   - Prepare customer communication

Remediation Steps:
# Remove public access
aws s3api put-public-access-block \
  --bucket example-com-backups \
  --public-access-block-configuration \
  "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"

# Remove bucket policy
aws s3api delete-bucket-policy --bucket example-com-backups

# Enable encryption
aws s3api put-bucket-encryption \
  --bucket example-com-backups \
  --server-side-encryption-configuration '{
    "Rules": [{
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "aws:kms",
        "KMSMasterKeyID": "arn:aws:kms:us-east-1:123456789:key/abc-123"
      }
    }]
  }'

# Enable logging
aws s3api put-bucket-logging \
  --bucket example-com-backups \
  --bucket-logging-status '{
    "LoggingEnabled": {
      "TargetBucket": "example-com-logs",
      "TargetPrefix": "backups-access/"
    }
  }'

Verification:
After remediation:
1. Attempt anonymous access (should fail)
2. Verify CloudTrail logging enabled
3. Confirm encryption at rest
4. Review IAM policies for least privilege
5. Test restore process with new encryption

References:
- OWASP: Sensitive Data Exposure
- CWE-200: Exposure of Sensitive Information
- AWS S3 Security Best Practices
- NIST SP 800-53: AC-3 (Access Enforcement)

Attachments:
- Screenshot: S3 bucket public access
- Log: Sample bucket listing output
- Evidence: Sanitized backup file header
```

### Visual Documentation

**Architecture Diagrams:**
Show attack paths and trust boundaries.

```
[Internet]
    |
    v
[Cloudflare CDN] <-- Trust Boundary 1: Public/DMZ
    |
    v
[AWS ALB]
    |
    v
[ECS Application Servers] <-- Trust Boundary 2: DMZ/Internal
    |
    +---> [RDS PostgreSQL] <-- Trust Boundary 3: App/Data
    +---> [ElastiCache Redis]
    +---> [S3 Buckets]
    |        |
    |        +---> [example-com-assets] (Public) ✓ Intentional
    |        +---> [example-com-backups] (Public) ✗ CRITICAL ISSUE
    |
    +---> [External APIs]
           +---> [Stripe API]
           +---> [SendGrid API]

Legend:
✓ = Secure configuration
✗ = Security issue
---> = Data flow
```

**Risk Matrix Visualization:**
Color-coded for impact.

**Network Topology:**
Show segmentation and access paths.

**Attack Trees:**
Visual representation of attack scenarios.

## Presentation

### Slide Structure

**15-30 slides total:**
```
1. Title / Cover
2. Agenda
3. Executive Summary
4. Scope and Methodology
5-7. Key Findings (1 per slide, top 3)
8. Risk Overview (dashboard/matrix)
9-11. Critical Risks Detail (1 per slide)
12-14. Recommendations (immediate/short/long)
15. Investment and ROI
16. Roadmap Timeline
17. Q&A
```

### Slide Content Guidelines

**Keep it visual:**
- One key point per slide
- Large, readable fonts
- Charts and graphs over tables
- Use red/yellow/green color coding
- Minimal text (speaker notes, not slides)

**Example Slide:**
```
-----------------------------------------
CRITICAL FINDING #1

Public Customer Database Backup

[Large icon of open S3 bucket]

250,000 customer records
Exposed to the internet
No authentication required

Impact: $385M+ potential exposure
Fix: 1 hour, $0
-----------------------------------------
```

### Storytelling Structure

**Problem → Impact → Solution**

```
Slide 1: The Problem
"We found customer data exposed on the internet"

Slide 2: The Impact
"This violates GDPR and could cost $385M+"

Slide 3: The Solution
"We can fix it in 1 hour with zero cost"

Slide 4: Why It Matters
"This protects 250k customers and avoids regulatory fines"
```

## Supporting Documentation

### Asset Inventory Spreadsheet

Provide detailed asset database.

**Format:**
- Excel or Google Sheets
- Multiple tabs (by asset type)
- Filterable columns
- Color-coding for risk levels

**Include:**
- All discovered assets
- Classifications and criticality
- Technologies and versions
- Ownership
- Findings associated with each asset

### Risk Register

Comprehensive risk tracking document.

**Use the format from Chapter 7:**
- Risk ID, Name, Description
- Likelihood and Impact scores
- Inherent and Residual risk
- Controls and mitigations
- Ownership and timelines
- Status tracking

### Remediation Roadmap

Gantt chart or timeline showing:
```
Q1 2026:
- [Week 1] Fix critical S3 bucket permissions
- [Week 1] Close exposed database ports
- [Week 2-3] Deploy rate limiting
- [Week 4-7] Implement MFA
- [Week 8-13] Deploy EDR

Q2 2026:
- Security awareness training rollout
- Network segmentation planning
- Vendor risk assessment program

Q3-Q4 2026:
- Network segmentation implementation
- Zero trust architecture (phase 1)
- Continuous monitoring deployment
```

### Evidence Package

Supporting materials:
- Screenshots of findings
- Tool output (sanitized)
- Configuration files (sanitized)
- Logs and proof of vulnerabilities
- References and citations

## Report Writing Best Practices

### Clarity

**Be specific:**
```
BAD:
"The system has security issues."

GOOD:
"The customer API at api.example.com has an IDOR vulnerability in the GET /users/{id} endpoint, allowing any authenticated user to access any other user's personal information."
```

**Avoid jargon (in executive sections):**
```
BAD:
"The IDOR in the RESTful API endpoint facilitates unauthorized data exfiltration via parameter manipulation."

GOOD:
"A security gap in the customer API allows users to access other customers' personal information by changing a number in the web request."
```

**Use active voice:**
```
BAD:
"It was found that the database is exposed."

GOOD:
"We found that the database is exposed to the internet."
```

### Actionability

Every finding needs:
- What's wrong
- Why it matters (impact)
- How to fix it (specific steps)
- Who should fix it
- When to fix it
- How to verify it's fixed

### Accuracy

**Verify everything:**
- Test findings before reporting
- Don't report false positives
- Distinguish between confirmed and potential
- Cite sources and references

**Confidence levels:**
```
Confirmed: Direct evidence of vulnerability
Likely: Strong indicators, not directly confirmed
Possible: Theoretical based on observations
Theoretical: No evidence, but could exist
```

### Professionalism

**Tone:**
- Objective and factual
- Not alarmist or inflammatory
- Not condescending
- Constructive, not critical

**BAD:**
"Your developers clearly don't understand security and have created a disaster waiting to happen."

**GOOD:**
"We identified several security gaps that require attention. These are common issues in rapidly developed applications and can be systematically addressed."

## Delivery and Review

### Draft Review Process

**Internal review:**
1. Technical review (accuracy check)
2. Editorial review (clarity and grammar)
3. Legal review (if needed for sensitive findings)
4. Final quality check

**Client draft review:**
1. Deliver draft report
2. Client reviews for accuracy
3. Clarification meeting if needed
4. Address factual errors or misunderstandings
5. Finalize report

**Don't change:**
- Findings based on evidence
- Risk ratings based on methodology
- Critical recommendations

**Do update:**
- Factual errors
- Asset ownership
- Timeline adjustments (if client has constraints)
- Add context you didn't have

### Presentation Delivery

**Prepare:**
- Anticipate questions
- Know your numbers
- Have backup slides for deep dives
- Practice timing

**During presentation:**
- Read the room
- Adjust detail level based on audience
- Focus on business impact for executives
- Offer to provide technical details separately

**Common questions:**
- "How did you find this?" (methodology)
- "Why didn't we catch this?" (no blame, explain)
- "How much will this cost?" (have numbers ready)
- "How long will this take?" (have timelines)
- "What should we do first?" (have priorities clear)

## Follow-Up

### Remediation Support

**Offer assistance:**
- Clarification on findings
- Technical guidance on fixes
- Verification testing after remediation
- Follow-up assessment (after fixes)

### Tracking Progress

**Status updates:**
- Monthly or quarterly check-ins
- Track remediation progress
- Update risk register
- Report on risk reduction

### Lessons Learned

**Debrief:**
- What went well
- What could improve
- Process improvements
- Tool effectiveness

## Output Checklist

Before delivery, verify:

```
[ ] Executive report (5-10 pages)
[ ] Technical report (50-200 pages)
[ ] Presentation (15-30 slides)
[ ] Asset inventory spreadsheet
[ ] Risk register
[ ] Remediation roadmap
[ ] Evidence package (if requested)
[ ] All findings verified
[ ] All recommendations actionable
[ ] All costs and timelines provided
[ ] All ownership assigned
[ ] Internal review completed
[ ] Grammar and spelling checked
[ ] Formatting consistent
[ ] Confidential information protected
[ ] Client branding applied (if required)
```

## Next Steps

Documentation complete. Reports ready for delivery.

Chapter 9 covers maintaining the threat model over time. Threat models aren't static - the environment changes, new threats emerge, controls are implemented. How to keep the threat model current and valuable.

You've built the model. Now keep it alive.