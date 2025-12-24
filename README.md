# Organizational Threat Modeling Manual

A comprehensive, practical guide for performing organizational threat modeling. Built for security consultants and internal security teams conducting enterprise-level threat assessments.

## What This Is

This manual covers the complete process of threat modeling an entire organization - not just an application, but every public asset, every employee, every vendor relationship, every piece of infrastructure.

You'll learn how to:
- Discover and enumerate an organization's complete digital footprint
- Build comprehensive asset inventories with proper classification
- Identify and profile relevant threat actors
- Map attack surfaces across external, internal, human, and supply chain vectors
- Apply formal threat modeling methodologies (STRIDE, PASTA, ATT&CK, etc.)
- Assess and quantify risk with actionable prioritization
- Document findings for different audiences (executives, engineers, compliance)
- Maintain threat models over time with continuous monitoring

This isn't theory. It's a field manual. Every chapter includes commands, examples, templates, and real-world guidance.

## Who This Is For

**Security Consultants:**
Use this as your engagement framework. Everything from scoping to delivery is documented, with templates you can copy directly into your work.

**Internal Security Teams:**
Build or improve your threat modeling program. The methodology scales from small startups to large enterprises.

**Security Engineers:**
Understand how threat modeling fits into the broader security program and how to contribute effectively.

**Risk Managers:**
Learn how to quantify and communicate security risk to business stakeholders.

## How To Use This Manual

**For new engagements:**
1. Start with [Chapter 1: Introduction](01-introduction.md) to scope and plan
2. Follow [Chapter 2: Reconnaissance](02-reconnaissance.md) for discovery
3. Work through Chapters 3-7 for analysis and risk assessment
4. Use [Chapter 8: Documentation](08-documentation.md) for reporting
5. Implement [Chapter 9: Maintenance](09-maintenance.md) for ongoing updates

**For specific needs:**
- Need threat modeling frameworks? → [Chapter 6: Methodologies](06-methodologies.md)
- Need to calculate risk scores? → [Chapter 7: Risk Assessment](07-risk-assessment.md)
- Need report templates? → [Chapter 8: Documentation](08-documentation.md) + [Appendix](appendix-templates.md)
- Need reconnaissance commands? → [Chapter 2: Reconnaissance](02-reconnaissance.md) + [Appendix](appendix-templates.md)

**For ongoing programs:**
Use [Chapter 9: Maintenance](09-maintenance.md) for continuous monitoring, quarterly updates, and keeping your threat model current.

## Manual Structure

### [Outline and Table of Contents](00-outline.md)
Complete overview of the manual structure and how chapters connect.

### [Chapter 1: Introduction and Methodology](01-introduction.md)
- What organizational threat modeling is and when to perform it
- Scope definition and boundaries
- Legal and ethical considerations
- Engagement planning and timelines
- Documentation requirements

### [Chapter 2: Reconnaissance and Discovery](02-reconnaissance.md)
- Domain and subdomain enumeration (passive and active)
- Infrastructure mapping (IP ranges, cloud resources, services)
- Technology stack identification
- Code repository discovery
- Employee and organizational intelligence (OSINT)
- Document and data leakage discovery
- Third-party and supply chain mapping
- Tools, techniques, and automation

### [Chapter 3: Asset Inventory and Classification](03-asset-inventory.md)
- Building structured asset databases
- Asset categorization frameworks
- Criticality assessment (Critical/High/Medium/Low)
- Data classification (Restricted/Confidential/Internal/Public)
- Dependency mapping and network topology
- Handling ambiguity and verification

### [Chapter 4: Threat Identification](04-threat-identification.md)
- Threat actor profiling (criminals, nation-states, insiders, etc.)
- Industry-specific threats
- Threat intelligence integration
- Historical incident analysis
- Mapping threats to assets
- Insider threat considerations

### [Chapter 5: Attack Surface Analysis](05-attack-surface.md)
- External attack surface mapping
- Internal attack surface (post-compromise scenarios)
- Human attack surface (phishing, social engineering)
- Supply chain attack surface
- Attack path modeling and kill chain analysis
- Attack surface reduction opportunities

### [Chapter 6: Threat Modeling Methodologies](06-methodologies.md)
- STRIDE (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege)
- PASTA (Process for Attack Simulation and Threat Analysis)
- Attack Trees and attack path visualization
- Kill Chain Analysis (Lockheed Martin Cyber Kill Chain)
- MITRE ATT&CK mapping and gap analysis
- OCTAVE (Operationally Critical Threat, Asset, and Vulnerability Evaluation)
- Choosing and combining methodologies

### [Chapter 7: Risk Assessment and Prioritization](07-risk-assessment.md)
- Risk fundamentals (Likelihood × Impact)
- Likelihood assessment with multiple factors
- Impact assessment (financial, operational, compliance, reputation)
- Risk matrices and scoring
- Quantitative methods (FAIR, Monte Carlo simulation)
- Risk treatment (Mitigate, Accept, Transfer, Avoid)
- Prioritization frameworks and cost-benefit analysis

### [Chapter 8: Documentation and Reporting](08-documentation.md)
- Multi-document approach (executive, technical, presentation)
- Executive report structure and business impact translation
- Technical report organization and finding formats
- Presentation delivery and storytelling
- Supporting documentation (asset inventory, risk register, roadmaps)
- Writing best practices and client communication

### [Chapter 9: Maintenance and Continuous Monitoring](09-maintenance.md)
- Why threat models become outdated
- Update triggers (scheduled, event-driven, continuous)
- Continuous asset discovery and monitoring
- Threat intelligence integration
- Metrics and KPIs for tracking progress
- Version control and change management
- Long-term sustainability

### [Appendix: Templates and Checklists](appendix-templates.md)
Ready-to-use templates:
- Reconnaissance checklist
- Asset inventory template
- Threat scenario template
- Risk register template
- Finding template
- Executive report template
- Tools reference and common commands
- Engagement checklist
- Risk matrix quick reference
- Compliance mapping

## Methodology

This manual uses a phased approach:

```
1. Reconnaissance → 2. Asset Inventory → 3. Threat Identification
                                              ↓
                    6. Documentation  ← 5. Risk Assessment ← 4. Attack Surface
                                              ↓
                              7. Maintenance (Continuous)
```

Each phase builds on the previous. You can't assess risk without understanding threats. You can't identify threats without knowing your assets. You can't catalog assets without discovery.

The manual supports multiple threat modeling frameworks - use what fits your engagement or combine approaches for comprehensive coverage.

## Philosophy

**Practical over theoretical:**
Every technique is explained with commands, examples, and real-world context. If you can't execute it, it's not in here.

**Actionable over comprehensive:**
Better to have 10 high-priority findings with remediation plans than 100 findings without context or prioritization.

**Business-focused over technical purism:**
Security exists to enable business. Risk assessment ties technical findings to business impact. Recommendations include cost and ROI.

**Living over static:**
Threat models decay. Chapter 9 covers keeping your assessment current through continuous monitoring and regular updates.

## Prerequisites

**Knowledge:**
- Basic understanding of networking, web applications, and common attack vectors
- Familiarity with command-line tools
- Understanding of basic security concepts

**Skills:**
- Comfortable with Linux/Unix command line
- Able to read and interpret technical documentation
- Basic scripting (bash, python) helpful but not required

**Tools:**
Most tools referenced are free and open source. See [Chapter 2](02-reconnaissance.md) and the [Appendix](appendix-templates.md) for complete tool lists and installation instructions.

## Time Investment

**Small organization** (under 100 employees): 40-80 hours
**Medium organization** (100-1000 employees): 80-160 hours
**Large organization** (1000+ employees): 160-400+ hours

These estimates include discovery, analysis, and documentation. Complex environments (multi-cloud, extensive SaaS, highly regulated) take longer.

## Version

Version 1.0.0 - Initial Release

## Changelog

See individual chapter files for detailed change history.

---

## Quick Start

1. Read the [Outline](00-outline.md) to understand the structure
2. Review [Chapter 1](01-introduction.md) for methodology and scope
3. Jump to the chapter that matches your current need
4. Use the [Appendix](appendix-templates.md) for templates and quick references
5. Adapt everything to your specific engagement

The manual is designed for non-linear use. Each chapter stands alone while contributing to the complete methodology.

Start wherever you need. The cross-references will guide you to related sections.

---

**Author Note:**

This manual represents years of organizational threat modeling experience distilled into a practical framework. It's not the only way to do threat modeling, but it's a proven approach that scales from startups to enterprises.

Use what works. Adapt what doesn't. Contribute improvements back.

The threat landscape evolves. So should your methodology.
