# Chapter 7: Risk Assessment and Prioritization

## Overview

You have a list of threats. Not all of them matter equally. A theoretical nation-state attack is less urgent than credential stuffing happening right now.

Risk assessment transforms threat models into actionable priorities. This chapter covers calculating risk scores, building risk matrices, and deciding what to fix first, what to accept, and what to transfer.

## Risk Fundamentals

### Risk Formula

```
Risk = Likelihood × Impact
```

Simple formula. Difficult execution.

**Likelihood:**
How probable is this threat?
- Historical data (has it happened before?)
- Threat actor capability and motivation
- Existing controls
- Attack surface exposure
- Industry trends

**Impact:**
What happens if the threat materializes?
- Financial loss
- Data breach consequences
- Regulatory penalties
- Reputation damage
- Operational disruption
- Legal liability

**Risk:**
The combination determines priority.
- High likelihood + High impact = Critical risk
- High likelihood + Low impact = Manage risk
- Low likelihood + High impact = Monitor risk
- Low likelihood + Low impact = Accept risk

### Risk vs. Vulnerability

**Vulnerability:**
A weakness in a system. A flaw. A gap.

Example: SQL injection vulnerability in login form.

**Threat:**
A potential danger. Someone who might exploit the vulnerability.

Example: Criminals exploiting SQLi to steal customer data.

**Risk:**
The actual business consequence if the threat exploits the vulnerability.

Example: $2M in damages from data breach, regulatory fines, and lawsuits.

Vulnerabilities without threats = low risk.
Threats without vulnerabilities = no risk.
Both together = calculate the risk.

## Likelihood Assessment

### Likelihood Scales

**Five-point scale:**
```
1 - Very Low (Rare)
- Extremely unlikely
- No historical precedent
- Requires significant resources or nation-state capability
- Strong controls in place
Example: Zero-day exploit against highly hardened system

2 - Low (Unlikely)
- Unlikely but possible
- Rare in industry
- Requires advanced skills
- Moderate controls in place
Example: APT targeting mid-size company

3 - Medium (Possible)
- Could happen
- Occasional industry occurrence
- Moderate skill required
- Some controls in place
Example: Targeted phishing campaign

4 - High (Likely)
- Likely to occur
- Common in industry
- Low skill required
- Weak or missing controls
Example: Credential stuffing against unprotected login

5 - Very High (Almost Certain)
- Actively occurring or will occur
- Constant industry occurrence
- Automated or trivial execution
- No effective controls
Example: Automated scanning and exploitation of known CVE
```

### Likelihood Factors

**Historical data:**
Has this happened before?
- To this organization (highest weight)
- To similar organizations (high weight)
- In this industry (medium weight)
- Anywhere (low weight)

If ransomware hit your organization last year, likelihood of another ransomware attack is HIGH, regardless of theoretical calculations.

**Threat actor motivation:**
Do attackers want this?

```
Asset: Customer database with PII

High Motivation:
- Criminals (sell data)
- Competitors (business intelligence)
- Nation-states (if strategically valuable)

Low Motivation:
- Hacktivists (unless controversial company)
- Script kiddies (no direct value)

Conclusion: HIGH motivation from primary threat actors
```

**Attack surface exposure:**
How accessible is the target?

```
Scenario: Exploit misconfigured S3 bucket

Exposure: Public internet
Authentication: None required
Discoverability: Easy (automated enumeration)
Complexity: Trivial (just access the URL)

Conclusion: VERY HIGH likelihood if bucket is misconfigured
```

**Existing controls:**
What's stopping the attack?

```
Threat: SQL injection against web application

Controls Present:
✓ WAF with SQLi rules (reduces likelihood)
✓ Input validation (reduces likelihood)
✓ Parameterized queries in most code (reduces likelihood)
✗ Legacy code still uses string concatenation (increases likelihood)
✗ No code review for SQL (increases likelihood)

Net Assessment: MEDIUM likelihood
- Multiple controls in place
- But gaps exist in legacy code
```

**Attacker capability required:**
How hard is this?

```
Attack: Credential stuffing
- Tools: Free and public
- Skills: Minimal (run a script)
- Resources: Cheap proxy IPs
- Time: Hours
Capability Required: LOW

Attack: Custom zero-day development
- Tools: Advanced development environment
- Skills: Expert-level (0day research)
- Resources: Significant ($$$)
- Time: Months
Capability Required: VERY HIGH

Lower capability requirement = Higher likelihood
```

**Industry trends:**
What's happening now?

```
2023-2024 Trends:
- Ransomware: VERY HIGH (constant)
- Supply chain attacks: HIGH (increasing)
- Cloud misconfigurations: VERY HIGH (constant)
- MFA fatigue attacks: MEDIUM (emerging)
- AI-powered attacks: LOW (experimental)

Adjust likelihood based on current threat landscape.
```

### Likelihood Calculation Example

```
Threat: Phishing Attack Against Employees

Factor Analysis:

Historical:
- No successful phishing in past year (but attempts observed)
- Industry: 75% of companies report phishing attempts
Weight: HIGH likelihood

Motivation:
- Criminals: HIGH (credential theft, initial access)
- APTs: MEDIUM (targeted attacks)
Weight: HIGH

Exposure:
- All employees have email (100% surface)
- Email addresses easily enumerated (LinkedIn, website)
Weight: VERY HIGH

Existing Controls:
- Email gateway with spam filtering (reduces)
- No anti-phishing training (increases)
- No phishing simulations (increases)
- Links and attachments not sandboxed (increases)
Weight: Control gaps = increases likelihood

Capability:
- Phishing kits: Free and available
- Skills: Minimal
- Success rate: 10-30% in untrained populations
Weight: LOW barrier = HIGH likelihood

Trends:
- Phishing constantly evolving
- AI-generated phishing improving
- Current active threat
Weight: VERY HIGH

Overall Likelihood: HIGH (4/5)

Rationale:
- Constant threat
- Easy to execute
- High exposure
- Control gaps
- Despite no recent success, conditions favor future success
```

## Impact Assessment

### Impact Categories

**Financial impact:**
Direct and indirect costs.

```
Direct Costs:
- Incident response ($50k - $500k+)
- Forensic investigation ($30k - $200k)
- Legal fees ($100k - $1M+)
- Regulatory fines (varies by regulation)
- Customer compensation (if contractual)
- Credit monitoring for affected individuals ($15-30 per person)

Indirect Costs:
- Lost revenue during downtime
- Customer churn (breach impact on retention)
- Increased insurance premiums
- Security improvements (post-incident)
- Lost business opportunities

Example Calculation:
Data breach exposing 100,000 customer records:
- Investigation: $150k
- Legal: $300k
- Regulatory fine (GDPR): Up to 20M EUR or 4% revenue
- Credit monitoring: $2M (100k × $20)
- Lost business: $5M (estimated)
- Total: $7.45M+ (not including fine)
```

**Operational impact:**
Business disruption.

```
Downtime Cost Formula:
Cost per hour = (Revenue per year) / (Hours per year)

Example:
$50M annual revenue / 8,760 hours = $5,700 per hour

Ransomware causing 72-hour outage:
72 hours × $5,700 = $410,400 in lost revenue
+ Recovery costs
+ Ransom decision (pay or rebuild)
+ Reputation impact on future sales

Partial Disruption:
If only 30% of operations affected:
$410,400 × 0.3 = $123,120
```

**Compliance and legal impact:**
Regulatory consequences.

```
Regulations and Penalties:

GDPR (EU):
- Up to 20M EUR or 4% of global revenue (whichever higher)
- Per incident
- Applies to EU data regardless of company location

PCI-DSS:
- Fines from payment brands: $5k-$100k per month until compliant
- Increased transaction fees
- Loss of ability to process cards (business ending)

HIPAA:
- $100-$50,000 per violation (max $1.5M per year)
- Criminal penalties possible

State Laws (CCPA, etc.):
- Statutory damages per record
- Class action lawsuits
- Attorney General enforcement

SOC2:
- Not regulatory but contractual
- Loss of certification = loss of enterprise customers
```

**Reputation impact:**
Trust and brand damage.

```
Quantifying Reputation:

Customer Trust Loss:
- 65% of breach victims lose trust in organization
- 30% stop doing business after breach
- Average customer lifetime value × churn rate = reputation cost

Example:
10,000 customers affected
30% churn = 3,000 lost customers
Customer LTV = $5,000
Reputation cost = $15M

Media Coverage:
- National breach coverage: High impact
- Industry coverage: Medium impact
- Local/minimal coverage: Low impact

Recovery Time:
- Trust restoration: 1-3 years
- Brand recovery: 2-5 years
- Some damage may be permanent
```

**Data sensitivity impact:**
What data was exposed?

```
Impact by Data Type:

Payment Card Data (PCI):
- CRITICAL impact
- Direct financial fraud
- Regulatory fines
- Card brand penalties
- Loss of processing ability

Health Information (HIPAA):
- CRITICAL impact
- Privacy violation
- Regulatory penalties
- Lawsuits
- Emotional harm to individuals

Personal Identifiable Information:
- HIGH impact
- Identity theft risk
- Regulatory penalties (GDPR, CCPA)
- Notification requirements
- Credit monitoring costs

Authentication Credentials:
- HIGH impact
- Account takeover
- Lateral movement
- Persistent access
- Downstream compromises

Proprietary Information:
- HIGH impact (if trade secrets)
- MEDIUM impact (if competitive intelligence)
- Competitive disadvantage
- Lost innovation value

Public Information:
- LOW impact
- Minimal business consequence
- Possible integrity concerns
```

### Impact Scales

**Five-point scale:**
```
1 - Very Low (Negligible)
Financial: <$10k
Operational: <1 hour disruption, minimal functions
Compliance: No violations
Reputation: No public awareness
Data: Public information only

2 - Low (Minor)
Financial: $10k-$100k
Operational: 1-8 hours disruption, non-critical functions
Compliance: Minor violations, no fines
Reputation: Limited awareness, quick recovery
Data: Internal information, no sensitive data

3 - Medium (Moderate)
Financial: $100k-$1M
Operational: 8-24 hours disruption, some critical functions
Compliance: Reportable violations, potential fines
Reputation: Industry awareness, months to recover
Data: Customer PII, some sensitive data

4 - High (Significant)
Financial: $1M-$10M
Operational: 1-7 days disruption, multiple critical functions
Compliance: Serious violations, significant fines
Reputation: National awareness, years to recover
Data: Large-scale PII, payment data, health information

5 - Very High (Catastrophic)
Financial: >$10M
Operational: >7 days disruption, business-critical failure
Compliance: Severe violations, massive fines, potential criminal charges
Reputation: Widespread awareness, permanent damage possible
Data: Massive breach of sensitive data, catastrophic exposure
```

### Impact Calculation Example

```
Threat: Data Breach via SQL Injection
Compromised: Customer database (250,000 records)

Financial Impact:
- Incident response: $200k
- Forensics: $150k
- Legal: $500k
- GDPR fine (potential): $10M (4% of $250M revenue)
- Credit monitoring: $5M (250k × $20)
- Lost business (est.): $8M
Total: $23.85M
Rating: VERY HIGH (5/5)

Operational Impact:
- Database taken offline for investigation: 48 hours
- Service degradation: 5 days
- Customer support overload: 2 weeks
- Revenue impact: $274k (48h × $5.7k/hr)
Rating: HIGH (4/5)

Compliance Impact:
- GDPR violation: Definite
- Mandatory breach notification
- Regulatory investigation
- Potential loss of certifications
Rating: VERY HIGH (5/5)

Reputation Impact:
- National media coverage (likely)
- Customer trust loss: ~30% churn
- Churn cost: 75k customers × $5k LTV = $375M
- Brand recovery: 3-5 years
Rating: VERY HIGH (5/5)

Data Impact:
- Customer PII exposed
- Email addresses (phishing target)
- Physical addresses (identity theft)
- Payment tokens (limited, tokenized)
- GDPR and CCPA scope
Rating: HIGH (4/5)

Overall Impact: VERY HIGH (5/5)
(Use highest rating from any category for overall impact)

Total Risk: HIGH Likelihood (4) × VERY HIGH Impact (5) = CRITICAL
```

## Risk Matrices

### Building Risk Matrices

**5x5 Matrix (most common):**
```
                          IMPACT
              Very Low  Low  Medium  High  Very High
           ┌─────────────────────────────────────────┐
Very High  │    L     │  M  │   H    │  H  │    C    │
           ├──────────┼─────┼────────┼─────┼─────────┤
    High   │    L     │  M  │   M    │  H  │    C    │
LIKELIHOOD ├──────────┼─────┼────────┼─────┼─────────┤
   Medium  │    L     │  L  │   M    │  M  │    H    │
           ├──────────┼─────┼────────┼─────┼─────────┤
    Low    │    L     │  L  │   L    │  M  │    M    │
           ├──────────┼─────┼────────┼─────┼─────────┤
 Very Low  │    L     │  L  │   L    │  L  │    L    │
           └─────────────────────────────────────────┘

L = Low Risk (Accept)
M = Medium Risk (Manage)
H = High Risk (Mitigate)
C = Critical Risk (Fix Immediately)
```

**Color coding:**
```
Green (Low): Accept or monitor
Yellow (Medium): Plan mitigation
Orange (High): Prioritize mitigation
Red (Critical): Immediate action required
```

### Populating the Matrix

```
Example Risk Matrix for Example Corp:

CRITICAL Risks (Immediate Action):
1. [4,5] Credential stuffing on customer portal
2. [5,4] Exposed database on legacy system
3. [4,5] Public S3 bucket with customer backups
4. [5,5] Ransomware (unpatched systems + no backups)

HIGH Risks (Prioritize):
5. [3,5] SQL injection in customer API
6. [4,4] Phishing without training or controls
7. [3,4] Insider data exfiltration
8. [4,3] DDoS against revenue-generating services

MEDIUM Risks (Plan):
9. [2,4] APT targeting intellectual property
10. [3,3] Third-party vendor compromise
11. [2,3] Subdomain takeover (limited impact)
12. [3,2] Information disclosure in error messages

LOW Risks (Monitor or Accept):
13. [1,3] Physical intrusion (low likelihood, good controls)
14. [2,2] Legacy system compromise (deprecated, no data)
15. [1,1] Theoretical attack requiring zero-day
```

## Quantitative Risk Assessment

### FAIR Methodology

Factor Analysis of Information Risk. Quantitative approach.

**Components:**
```
Risk = Probable Frequency × Probable Magnitude

Probable Frequency = Threat Event Frequency × Vulnerability

Probable Magnitude = Primary Loss + Secondary Loss

Primary Loss:
- Productivity
- Response costs
- Replacement costs

Secondary Loss:
- Fines and judgments
- Competitive advantage loss
- Reputation damage
```

**Example Calculation:**
```
Scenario: Ransomware Attack

Threat Event Frequency:
Based on industry data and organizational profile:
- Attempts per year: 12 (monthly targeting)
- Successful compromise rate: 15% (phishing success)
- Frequency: 1.8 events per year

Vulnerability:
- Probability attack succeeds given attempt: 60%
  (Based on: Weak backups, unpatched systems, no EDR)

Loss Event Frequency:
1.8 × 0.60 = 1.08 events per year (~1 successful ransomware per year)

Primary Loss Magnitude:
- Incident response: $150k
- Downtime (72h): $410k
- Recovery/rebuild: $200k
- Ransom (if paid): $500k (average)
Total Primary: $1.26M

Secondary Loss Magnitude:
- Regulatory fines: $100k (notification violations)
- Lost business: $2M (customer churn)
- Reputation damage: $1M
- Insurance premium increase: $50k/year × 3 years = $150k
Total Secondary: $3.25M

Total Loss per Event: $4.51M

Annual Loss Expectancy (ALE):
Loss Event Frequency × Total Loss
1.08 × $4.51M = $4.87M per year

Mitigation Investment:
If implementing controls costs $500k:
ROI = ($4.87M - reduced ALE) - $500k

If controls reduce frequency to 0.1 events/year:
New ALE = 0.1 × $4.51M = $451k
ROI = ($4.87M - $451k) - $500k = $3.92M

Mitigation justified: YES
```

### Monte Carlo Simulation

For complex scenarios with uncertain variables.

```
Variables:
- Attack frequency: 1-4 per year (triangular distribution)
- Success rate: 10-30% (uniform distribution)
- Impact: $2M-$8M (log-normal distribution)

Run 10,000 simulations:
Results:
- Mean annual loss: $4.2M
- 90th percentile: $7.5M
- 95th percentile: $9.8M
- 99th percentile: $15M

Interpretation:
- Expected loss: $4.2M/year
- Budget for worst case: $10M (95th percentile)
- Insurance coverage: $15M (99th percentile)
```

## Prioritization Frameworks

### MoSCoW Method

Categorize risks:

**Must Fix:**
- Critical risks
- Regulatory requirements
- Active exploitation
- Severe business impact

**Should Fix:**
- High risks
- Industry best practices
- Moderate business impact
- Significant vulnerability

**Could Fix:**
- Medium risks
- Nice-to-have improvements
- Low business impact
- Opportunistic fixes

**Won't Fix (Now):**
- Low risks
- Resource constraints
- Accepted risks
- Deferred to future

### Risk Appetite

Define what risks the organization accepts.

```
Risk Appetite Statement:

Example Corp Risk Appetite:

ZERO tolerance for:
- Payment card data exposure
- GDPR violations
- Customer PII breaches
- Revenue-impacting outages >4 hours

LOW tolerance for:
- Compliance violations (non-GDPR)
- Operational disruptions
- IP theft
- Reputation damage

MODERATE tolerance for:
- Public information disclosure
- Non-critical system compromise
- Minor service degradations

Accept:
- Theoretical low-likelihood scenarios
- Residual risk after reasonable controls
- Risks below $100k impact
```

### Cost-Benefit Analysis

```
Risk: SQL Injection in Customer API
Current Risk: HIGH likelihood × VERY HIGH impact = CRITICAL
Annual Loss Expectancy: $2.3M

Mitigation Option 1: WAF Enhancement
Cost: $50k/year
Risk Reduction: 60% (ALE becomes $920k)
Net Benefit: $1.38M - $50k = $1.33M/year
ROI: 2,660%

Mitigation Option 2: Code Rewrite
Cost: $300k (one-time) + $30k/year maintenance
Risk Reduction: 95% (ALE becomes $115k)
Net Benefit: $2.185M - $30k = $2.155M/year
ROI: Payback in 2 months, then $2.155M/year

Recommendation: Option 2
- Higher upfront cost but superior risk reduction
- Permanent fix vs ongoing WAF dependency
- Better long-term ROI
```

## Risk Treatment Options

### Mitigate

Implement controls to reduce risk.

```
Risk: Phishing Attacks
Treatment: Mitigate

Controls to Implement:
1. Email security gateway ($30k/year)
   - Reduces likelihood by 40%

2. Security awareness training ($20k/year)
   - Reduces likelihood by 50%

3. Phishing simulations ($15k/year)
   - Reduces likelihood by 30%

4. MFA enforcement (no cost, already licensed)
   - Reduces impact by 70% (limits access even if phished)

Combined Effect:
- Likelihood: HIGH (4) → LOW (2)
- Impact: HIGH (4) → MEDIUM (3)
- Risk: CRITICAL → MEDIUM
- Cost: $65k/year
- ALE Reduction: $3.2M → $600k = $2.6M savings
- Net Benefit: $2.535M/year
```

### Accept

Acknowledge risk and take no action.

```
Risk: Theoretical Zero-Day Against Hardened System
Likelihood: Very Low (1)
Impact: High (4)
Risk: LOW-MEDIUM

Acceptance Rationale:
- Extremely low probability
- Mitigation cost ($500k for additional controls) exceeds expected loss
- Existing controls are strong
- No cost-effective mitigation available
- Monitor threat landscape for changes

Acceptance Criteria:
- Annual review
- Reassess if threat landscape changes
- Incident response plan includes this scenario
- Cyber insurance covers potential loss

Accepted by: CISO
Date: 2025-12-23
Review Date: 2026-12-23
```

### Transfer

Shift risk to third party (insurance, outsourcing).

```
Risk: Ransomware Impact
Residual Risk After Controls: MEDIUM
Potential Impact: $2M-$5M

Transfer Mechanism: Cyber Insurance

Policy Coverage:
- Ransomware payment: Up to $2M
- Business interruption: Up to $1M
- Incident response: Up to $500k
- Legal and regulatory: Up to $1M
- Notification costs: Up to $500k
Total Coverage: $5M

Premium: $75k/year
Deductible: $50k

Analysis:
- Transfers financial impact beyond $50k deductible
- Doesn't eliminate risk, only transfers financial impact
- Must still prevent and respond to incidents
- Reduces financial uncertainty
- Enables budget planning

Decision: Transfer
- Maintain strong controls (reduces premium)
- Accept deductible risk
- Transfer impact beyond deductible
```

### Avoid

Eliminate the risk entirely.

```
Risk: Data Breach of Legacy System
Current Risk: HIGH

Avoidance Option: Decommission Legacy System

Analysis:
- System contains customer data
- Outdated technology (unpatched, unsupported)
- Difficult and expensive to secure
- Functionality available in modern system

Avoidance Plan:
1. Migrate data to new system (8 weeks, $150k)
2. Redirect users to new system (2 weeks, $20k)
3. Decommission old system (1 week, $10k)
4. Total: 11 weeks, $180k

Risk After Avoidance: ELIMINATED
- No system = no risk
- Data migrated securely
- Attack surface reduced

Decision: Avoid
- One-time cost eliminates ongoing risk
- Simplifies architecture
- Reduces technical debt
```

## Residual Risk

Risk remaining after controls are applied.

```
Initial Risk Assessment:
Threat: Credential Stuffing
Likelihood: Very High (5)
Impact: High (4)
Inherent Risk: CRITICAL

Planned Mitigations:
1. Rate limiting (reduces likelihood to 3)
2. CAPTCHA (reduces likelihood to 2)
3. MFA (reduces impact to 2)

Residual Risk:
Likelihood: Low (2)
Impact: Low (2)
Residual Risk: LOW

Acceptance Decision:
- Residual risk within risk appetite
- Cost of additional controls exceeds benefit
- Accept residual risk with monitoring
```

## Risk Register

Maintain comprehensive risk register.

**Fields:**
```
Risk ID: Unique identifier
Risk Name: Descriptive name
Asset(s): Affected assets
Threat Actor: Who poses this threat
Attack Vector: How they would attack
Inherent Likelihood: Before controls (1-5)
Inherent Impact: Before controls (1-5)
Inherent Risk: Likelihood × Impact
Current Controls: Existing mitigations
Control Effectiveness: How well controls work
Residual Likelihood: After controls (1-5)
Residual Impact: After controls (1-5)
Residual Risk: Likelihood × Impact
Risk Treatment: Mitigate/Accept/Transfer/Avoid
Planned Mitigations: Future controls
Target Likelihood: After planned mitigations
Target Impact: After planned mitigations
Target Risk: Final acceptable risk level
Owner: Who owns this risk
Status: Open/In Progress/Closed/Accepted
Review Date: When to reassess
```

**Sample Entry:**
```
Risk ID: R-047
Risk Name: SQL Injection in Customer API
Asset(s): api.example.com, customer database
Threat Actor: Opportunistic attackers, criminals
Attack Vector: Malicious input to API parameters

Inherent Likelihood: 4 (High)
Inherent Impact: 5 (Very High)
Inherent Risk: CRITICAL (20)

Current Controls:
- WAF with SQLi rules (partial)
- Input validation (inconsistent)
- Parameterized queries (80% coverage)

Control Effectiveness: MEDIUM (reduce likelihood 4→3)

Residual Likelihood: 3 (Medium)
Residual Impact: 5 (Very High)
Residual Risk: HIGH (15)

Risk Treatment: MITIGATE

Planned Mitigations:
1. Code review for SQLi (complete 80%→100% parameterization)
2. WAF rule enhancement
3. Database activity monitoring (detect exploitation)
4. Automated security testing in CI/CD

Target Likelihood: 1 (Very Low)
Target Impact: 3 (Medium - with monitoring, limit impact)
Target Risk: LOW (3)

Owner: VP Engineering
Status: In Progress (60% complete)
Next Review: 2026-01-15
Budget: $120k
Timeline: Q1 2026
```

## Communicating Risk

### Executive Communication

**Focus on business impact:**
```
BAD:
"We have a SQL injection vulnerability in the API."

GOOD:
"A security gap in our customer API could expose 250,000 customer records, resulting in $10M+ in regulatory fines, lawsuits, and lost business. We recommend $120k investment to fix it."
```

**Use visuals:**
- Risk matrices (color-coded)
- Trend charts (risk over time)
- Cost-benefit graphs
- Comparison to industry benchmarks

**Provide options:**
- Multiple mitigation approaches
- Cost-benefit for each
- Timeline and resources required
- Residual risk for each option

### Technical Communication

**Detailed threat scenarios:**
- Attack paths
- Technical controls
- Detection mechanisms
- Response procedures

**Specific mitigations:**
- Configuration changes
- Code fixes
- Architecture updates
- Tool deployments

## Risk Monitoring

Track risk over time.

**Metrics:**
```
Risk Metrics:

Total Risks: 127
- Critical: 4 (down from 8 last quarter)
- High: 23 (down from 31)
- Medium: 67 (stable)
- Low: 33 (up from 28)

Trend: IMPROVING

Risk Velocity:
- New risks identified: 12
- Risks closed: 16
- Risks accepted: 5
- Net change: -4 (good)

Top Risks (by ALE):
1. Ransomware: $4.8M/year
2. Data breach: $2.3M/year
3. DDoS: $1.2M/year

Mitigation Progress:
- On track: 45 risks
- Behind schedule: 8 risks
- Blocked: 2 risks (resource constraints)
```

## Next Steps

Risk assessed. Priorities established. Mitigation plans ready.

Chapter 8 covers documentation and reporting - packaging all this analysis into deliverables for stakeholders. Executive summaries, technical findings, remediation roadmaps.

You have the risk story. Now write it up.