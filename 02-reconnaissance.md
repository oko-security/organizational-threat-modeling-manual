# Chapter 2: Reconnaissance and Discovery

## Overview

Reconnaissance is where you build the foundation. Every asset you miss here is a blind spot in your threat model. Every piece of information you collect becomes context for understanding risk.

This chapter covers systematic discovery of an organization's public footprint. Work methodically. Document everything. The goal is completeness, not speed.

## Domain and Subdomain Enumeration

Start with what you know - the primary domain. Everything else branches from here.

### Primary Domain Discovery

**Known information gathering:**
- Corporate website domain
- Product domains
- Support and documentation domains
- Regional variants

**Additional domain discovery:**
```bash
# WHOIS history for registrant
whois example.com

# Look for pattern in registrant email
# If domains registered to security@example.com, search for other domains
# registered to same contact

# Search for organization name in domain registries
# Use DomainTools, SecurityTrails, or similar
```

**Alternate TLDs:**
Don't assume .com is the only one. Check:
- .net, .org, .io, .co
- Country codes (.uk, .de, .ca, etc.)
- New gTLDs (.app, .dev, .cloud, .tech)

**Acquisitions and subsidiaries:**
Research the organization's history:
- Recent acquisitions (may not be rebranded yet)
- Subsidiary companies
- Product-specific brands
- Historical company names (pre-rebrand domains)

Check:
- Company about pages and press releases
- Crunchbase and similar business databases
- LinkedIn company relationships
- SEC filings for public companies

### Subdomain Enumeration

This is critical. Subdomains often expose more than the main domain.

**Certificate transparency logs:**
```bash
# Using crt.sh
curl -s "https://crt.sh/?q=%25.example.com&output=json" | jq -r '.[].name_value' | sort -u

# Using certspotter
certspotter example.com

# Web interfaces
# crt.sh
# censys.io
# shodan.io
```

Certificate transparency is gold. Companies can't hide domains if they have valid SSL certificates.

**DNS enumeration tools:**
```bash
# subfinder - fast, uses multiple sources
subfinder -d example.com -o subdomains.txt

# amass - comprehensive, slower
amass enum -d example.com -o amass-results.txt

# assetfinder - simple, effective
assetfinder --subs-only example.com

# Combine results
cat subdomains.txt amass-results.txt assetfinder-results.txt | sort -u > all-subdomains.txt
```

**DNS brute force (when needed):**
```bash
# fierce - DNS reconnaissance
fierce --domain example.com

# dnsrecon
dnsrecon -d example.com -t brt -D /path/to/wordlist.txt

# puredns - fast mass DNS resolution
puredns bruteforce wordlist.txt example.com
```

Use brute force when passive enumeration seems incomplete. Common patterns:
- dev, staging, test, qa, uat
- admin, portal, dashboard, console
- api, api-dev, api-staging
- mail, smtp, webmail
- vpn, remote, rdp
- internal, intranet, corp
- Region/city names
- Product names
- Department names

**DNS zone transfers (rare but check):**
```bash
# Attempt zone transfer
dig axfr @ns1.example.com example.com
host -t axfr example.com ns1.example.com
```

Zone transfers are mostly locked down, but legacy DNS servers occasionally allow them.

**Subdomain analysis:**
Once you have subdomains, probe them:
```bash
# Check which are live
httpx -l subdomains.txt -o live-subdomains.txt

# Get status codes, titles, tech
httpx -l subdomains.txt -title -tech-detect -status-code -o detailed-results.txt

# Screenshot live hosts
gowitness file -f live-subdomains.txt
```

**Interesting subdomain patterns:**

Look for:
- Development/staging environments (often less secured)
- Administrative interfaces
- Legacy systems (old., legacy., v1., etc.)
- Geographic variants (us., eu., asia.)
- Vendor integrations (vendorname.example.com)
- Internal-sounding names (may be exposed by mistake)
- S3 buckets referenced in subdomains

**Subdomain takeover check:**
```bash
# subjack
subjack -w subdomains.txt -t 20 -timeout 30 -o takeovers.txt

# Check for dangling DNS records pointing to:
# - Deleted cloud resources (S3, Azure, Heroku)
# - Expired third-party services
# - Unclaimed GitHub Pages
```

Document any potential takeovers separately - high severity finding.

## Infrastructure Mapping

Map the network infrastructure behind the domains.

### IP Address Discovery

**DNS resolution:**
```bash
# Resolve all discovered domains/subdomains
while read domain; do
  dig +short $domain | grep -E '^[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+$'
done < all-domains.txt | sort -u > ip-addresses.txt
```

**WHOIS and ASN lookup:**
```bash
# Find ASN (Autonomous System Number)
whois -h whois.cymru.com " -v $(cat ip-addresses.txt | head -1)"

# Get netblocks for organization
# Use bgp.he.net or similar BGP looking glass
# Search for organization name to find ASNs
# Then get all prefixes for those ASNs
```

**Cloud provider identification:**
```bash
# AWS IP ranges
curl -s https://ip-ranges.amazonaws.com/ip-ranges.json | jq '.prefixes[] | select(.service=="AMAZON")'

# Azure IP ranges
# https://www.microsoft.com/en-us/download/details.aspx?id=56519

# GCP IP ranges
# https://www.gstatic.com/ipranges/cloud.json

# Cross-reference discovered IPs with cloud provider ranges
```

**Reverse DNS:**
```bash
# Reverse lookup on discovered IPs
for ip in $(cat ip-addresses.txt); do
  host $ip
done

# Look for patterns in PTR records
# May reveal additional infrastructure
```

### Port and Service Enumeration

**Port scanning:**
```bash
# Fast scan of common ports
nmap -iL ip-addresses.txt -T4 -Pn -p 80,443,8080,8443,22,21,3389 -oA nmap-quick

# More thorough scan
nmap -iL ip-addresses.txt -T4 -Pn -p- -oA nmap-full

# Service and version detection
nmap -iL ip-addresses.txt -sV -sC -p $(cat discovered-ports.txt) -oA nmap-services
```

Be mindful of rate limiting and IDS detection. Slow scans down if needed.

**Service fingerprinting:**
- SSH versions (often reveal OS and patch level)
- Web server banners
- Mail server software
- FTP banners
- Database ports (3306, 5432, 27017, etc.)

**Look for dangerous exposures:**
- RDP (3389) or VNC exposed to internet
- Database ports publicly accessible
- Elasticsearch, MongoDB, Redis without auth
- Admin panels (Jenkins, Kubernetes dashboard, etc.)
- Printer/IoT management interfaces
- IPMI, iDRAC, iLO (server management)

### Shodan and Mass Scanning Services

**Shodan queries:**
```
# Search by organization name
org:"Company Name"

# Search by domain
hostname:example.com

# Search by netblock
net:203.0.113.0/24

# Combine filters
org:"Company Name" port:3389

# Look for specific services
org:"Company Name" product:"Apache"
```

**Censys searches:**
Similar to Shodan but different data sources. Check both.

**What to look for:**
- Exposed services on unusual ports
- Vulnerable software versions
- Devices with default credentials
- Industrial control systems
- IoT devices
- VPN concentrators
- Security appliances

## Technology Stack Identification

Understanding the tech stack informs threat modeling. Different technologies have different vulnerability patterns.

### Web Application Fingerprinting

**HTTP headers:**
```bash
# Grab headers
curl -I https://example.com

# Look for:
# Server: nginx/1.18.0
# X-Powered-By: PHP/7.4.3
# X-AspNet-Version: 4.0.30319
# X-Generator: Drupal 9
```

**Automated fingerprinting:**
```bash
# whatweb
whatweb https://example.com

# wappalyzer-cli
wappalyzer https://example.com

# webanalyze
webanalyze -host https://example.com
```

**Manual inspection:**
- View page source for meta tags
- Check JavaScript files (framework signatures)
- Look at URL patterns (/wp-content = WordPress, etc.)
- Inspect cookies (session cookie names reveal tech)
- Check favicon.ico hash against known framework favicons

**Technology indicators:**

WordPress:
- /wp-content/, /wp-includes/ paths
- wp-json API endpoint
- Login at /wp-admin or /wp-login.php

Drupal:
- /node/, /user/ paths
- X-Generator header
- update.php, install.php files

Joomla:
- /administrator/ path
- Joomla meta tags
- /components/, /modules/ directories

Common frameworks:
- React: Look for react.js, react-dom.js
- Angular: ng-app, ng-controller in HTML
- Vue.js: vue.js, v-bind attributes
- Django: csrftoken cookie, __debug__ URLs
- Rails: Rails-specific headers, asset pipeline paths
- Laravel: laravel_session cookie

**Version identification:**
Knowing exact versions is crucial for vulnerability research.

Check:
- README, CHANGELOG files
- /version, /VERSION endpoints
- JavaScript source map files
- Package.json if exposed
- Error messages (often show versions)
- Default files with version info

### Backend and Database Detection

**Error-based fingerprinting:**
Trigger errors and read messages:
- SQL errors reveal database type and version
- Stack traces show framework and libraries
- 404 pages often show server software

**Timing attacks for database detection:**
```sql
# MySQL sleeps
' AND SLEEP(5)--

# PostgreSQL
' AND pg_sleep(5)--

# MSSQL
'; WAITFOR DELAY '00:00:05'--
```

Time responses to identify database. Don't actually inject - just understand the pattern for when you see it.

**Database port exposure:**
If you found open database ports:
- MySQL: 3306
- PostgreSQL: 5432
- MongoDB: 27017
- Redis: 6379
- Elasticsearch: 9200
- Cassandra: 9042

Test authentication requirements and document findings.

### Mail Infrastructure

**MX records:**
```bash
# Get mail servers
dig MX example.com

# Common providers:
# Google Workspace: aspmx.l.google.com
# Microsoft 365: *.mail.protection.outlook.com
# Proofpoint: *.pphosted.com
# Mimecast: *.mimecast.com
```

**SPF records:**
```bash
# Check sender policy framework
dig TXT example.com | grep "v=spf1"

# SPF includes reveal mail infrastructure
# ip4: entries show mail server IPs
# include: entries show third-party senders
```

**DMARC:**
```bash
# DMARC policy
dig TXT _dmarc.example.com

# Shows email authentication policy
# p=none (monitoring), p=quarantine, p=reject
```

**DKIM:**
```bash
# Common DKIM selectors
for selector in default google s1 s2 k1 k2 dkim; do
  dig TXT ${selector}._domainkey.example.com
done
```

**Mail server fingerprinting:**
```bash
# Connect to mail server
telnet mail.example.com 25

# Banner reveals software
# 220 mail.example.com ESMTP Postfix
# 220 mail.example.com ESMTP Exim
# 220-mail.example.com ESMTP Microsoft Exchange
```

### DNS and Name Servers

**NS records:**
```bash
dig NS example.com

# Common providers:
# Cloudflare: ns1.cloudflare.com
# AWS Route53: ns-*.awsdns-*.com
# Azure DNS: ns1-*.azure-dns.com
# Google Cloud DNS: ns-cloud-*.googledomains.com
```

**DNS provider reveals infrastructure:**
- Cloudflare: Using CDN and DDoS protection
- AWS Route53: Likely using AWS for hosting
- Authoritative on own infrastructure: Self-managed

**DNSSEC:**
```bash
dig +dnssec example.com

# RRSIG records indicate DNSSEC is enabled
```

### Cloud and Hosting Detection

**IP-based detection:**
Map IPs to cloud providers using ASN or IP range databases.

**AWS-specific discovery:**
```bash
# S3 bucket enumeration
# Common naming patterns:
# example-com, example-assets, example-backups
# company-production, company-staging

# Try to access buckets
aws s3 ls s3://example-com --no-sign-request

# Check for listable buckets
curl http://example-com.s3.amazonaws.com/

# Bucket finder tools
bucket_finder.rb example.com

# Common bucket names:
# [company]-[environment]
# [company]-assets
# [company]-backups
# [company]-logs
# [product]-[environment]
```

**Azure discovery:**
```bash
# Azure blob storage
# https://[account].blob.core.windows.net/

# Common account names match company name
curl https://examplecom.blob.core.windows.net/?comp=list
```

**GCP discovery:**
```bash
# Google Cloud Storage
# gs://[bucket-name]/
# https://storage.googleapis.com/[bucket-name]/

# Try common bucket names
curl https://storage.googleapis.com/example-com/
```

**Cloudflare detection:**
```bash
# Check if using Cloudflare
dig example.com

# If IPs in Cloudflare range, may be proxied
# Try to find origin IP:
# - Historical DNS records (SecurityTrails)
# - Subdomain without Cloudflare proxy
# - Mail servers often show real IP
# - SSL certificate SANs
```

**CDN usage:**
- Cloudflare, Akamai, Fastly, Amazon CloudFront
- Check HTTP headers (CF-Ray, X-Akamai-*, X-Cache)
- CNAME records pointing to CDN providers
- Assets loaded from CDN domains

## Code Repository Discovery

Public code leaks everything - infrastructure details, API keys, architecture decisions.

### GitHub Organization Discovery

**Find organization account:**
```bash
# Search for organization
# https://github.com/[company-name]
# Try variations: lowercase, no spaces, abbreviations

# Check users for organization membership
# Look at employee profiles
# Check repository contributors
```

**Repository enumeration:**
```bash
# Use GitHub API
curl "https://api.github.com/orgs/company-name/repos?per_page=100"

# Use github-search
gh repo list company-name --limit 1000

# Check forks and mirrors
# Often contain sensitive development branches
```

**Personal repositories:**
Employee personal accounts often have work code:
- Find employees via LinkedIn
- Search GitHub for their profiles
- Check repositories for company-related code
- Look at commit history for work patterns

**Sensitive file patterns:**
```bash
# Clone repositories and search for:
.env
.env.local
.env.production
config/database.yml
config/secrets.yml
credentials.json
serviceAccount.json
.aws/credentials
.ssh/id_rsa
*.pem
*.key
*.p12
```

**GitHub dorking:**
```
# Search code across GitHub
"company.com" password
"company.com" api_key
"company.com" secret
org:company-name filename:.env
org:company-name extension:pem
org:company-name filename:id_rsa
```

### GitLab and Bitbucket

**GitLab groups:**
```
https://gitlab.com/company-name
https://gitlab.com/explore/groups
```

**Bitbucket workspaces:**
```
https://bitbucket.org/company-name/
```

Same approach as GitHub - enumerate repositories, search for sensitive data.

### Gists and Paste Sites

**GitHub Gists:**
```bash
# Search Gists
# https://gist.github.com/search?q=example.com

# Check employee Gists
# Often contain quick scripts and config snippets
```

**Pastebin and similar:**
- pastebin.com
- paste.ee
- ghostbin.com
- privatebin variants

Search for:
- Domain name
- Company name
- IP addresses
- Employee email addresses

### Package Registries

**NPM packages:**
```bash
# Search npm
npm search @company-name

# Check package.json for internal packages
# May reveal architecture and dependencies
```

**PyPI:**
```bash
# Search Python Package Index
# https://pypi.org/search/?q=company-name

# Internal packages sometimes published by accident
```

**Maven/Gradle:**
- Search Maven Central
- Check for organizational group IDs
- Look for internal artifacts

**Docker Hub:**
```bash
# Search for organization
# https://hub.docker.com/u/company-name

# Public images may contain:
# - Configuration details
# - Internal tool versions
# - Base images revealing stack
```

**RubyGems, Cargo, NuGet:**
Same pattern - search for company-specific packages.

## Employee and Organizational Intelligence

People are part of the attack surface.

### LinkedIn Reconnaissance

**Employee enumeration:**
```
# LinkedIn search
site:linkedin.com/in "Company Name"

# Current employees
site:linkedin.com/in "Company Name" "present"

# Recent hires (within 6 months)
# Indicates growth areas or new initiatives
```

**Gather information:**
- Employee count and growth rate
- Office locations
- Department structure (Engineering, Security, IT, etc.)
- Job titles and hierarchy
- Technologies mentioned in profiles
- Certifications (AWS, Azure, Kubernetes, etc.)

**Target profile identification:**
- IT and Security leadership (CSO, CISO, CTO)
- System administrators
- Cloud engineers
- Security engineers
- Developers with access to production

**Technology intelligence:**
```
# Search employee profiles for technology mentions
site:linkedin.com/in "Company Name" "AWS"
site:linkedin.com/in "Company Name" "Kubernetes"
site:linkedin.com/in "Company Name" "Azure"
site:linkedin.com/in "Company Name" "Salesforce"
```

Technologies frequently mentioned indicate organizational tech stack.

### Job Posting Analysis

**Current openings:**
Job descriptions leak infrastructure details:

Look for:
- Required skills (Python, Go, Java = likely stack)
- Tools mentioned (Jenkins, GitLab, Terraform, Ansible)
- Cloud platforms (AWS, GCP, Azure)
- Security tools (Splunk, CrowdStrike, Okta)
- Compliance requirements (SOC2, ISO27001, HIPAA)
- Programming languages
- Frameworks and libraries

**Example findings from job post:**
```
"Senior DevOps Engineer required. Must have experience with:
- AWS (EC2, S3, RDS, Lambda)
- Kubernetes and Docker
- Terraform for infrastructure as code
- GitLab CI/CD
- DataDog for monitoring"

Findings:
- Cloud: AWS (confirmed)
- Container orchestration: Kubernetes
- IaC: Terraform (infrastructure is code-defined)
- CI/CD: GitLab
- Monitoring: DataDog
- Likely microservices architecture
```

### Social Media and Public Presence

**Twitter/X:**
- Corporate accounts (announcements, incidents)
- Employee accounts (tech discussions, conference talks)
- Search for domain name and company name
- Look for incident discussions

**Technical blogs:**
Many companies run engineering blogs:
- Medium publications
- dev.to accounts
- Company blog at /blog or blog.company.com

Articles often discuss:
- Architecture decisions
- Technology migrations
- Scaling challenges
- Tools and frameworks

**Conference talks and presentations:**
- YouTube for recorded talks
- SlideShare, SpeakerDeck for slides
- Conference websites for speaker bios

Engineers discussing their work reveal architecture and tools.

**Podcasts and interviews:**
Search for:
- Company name + podcast
- Executive interviews
- Engineering discussions

### Business Intelligence

**Company information:**
- Crunchbase (funding, acquisitions, investors)
- PitchBook (private company data)
- Glassdoor (employee reviews - sometimes mention tools)
- BuiltWith (technology usage statistics)

**Public companies:**
SEC filings reveal:
- Major vendors and contracts
- Risk factors (including security risks)
- Infrastructure investments
- M&A activity

## Document and Data Leakage

People accidentally expose sensitive information constantly.

### Search Engine Reconnaissance

**Google dorking:**
```
# Indexed files
site:example.com filetype:pdf
site:example.com filetype:xlsx
site:example.com filetype:docx
site:example.com filetype:pptx

# Configuration files
site:example.com ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:ini

# Database files
site:example.com ext:sql | ext:dbf | ext:mdb

# Backup files
site:example.com ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup

# Directory listings
site:example.com intitle:"index of"

# Login pages
site:example.com inurl:login
site:example.com inurl:admin
site:example.com inurl:portal

# Error messages
site:example.com intext:"sql syntax near"
site:example.com intext:"syntax error has occurred"
site:example.com intext:"warning: mysql"

# Confidential documents
site:example.com intext:confidential
site:example.com "internal use only"
```

**Historical content:**
```
# Google Cache
cache:example.com

# Wayback Machine
# https://web.archive.org/web/*/example.com
```

Historical snapshots sometimes contain data removed from live sites.

### Exposed Documents

**Document metadata:**
Downloaded documents contain metadata:
```bash
# Extract PDF metadata
exiftool document.pdf

# Look for:
# - Author names
# - Software versions
# - File paths (reveal internal structure)
# - Creation dates
# - Internal network names
```

**Content analysis:**
Documents often contain:
- Internal IP addresses
- Hostnames and server names
- Email addresses
- Organizational charts
- Architecture diagrams
- Configuration details

### Third-Party Exposure

**Pastebin reconnaissance:**
```
# Search pastebin
site:pastebin.com "example.com"
site:pastebin.com "Company Name"

# Check for:
# - Credential dumps
# - Configuration files
# - Database dumps
# - API keys
# - Email lists
```

**Breach databases:**
```
# HaveIBeenPwned
# Search for company domain
# https://haveibeenpwned.com/

# Breach compilations
# Search for @example.com in known dumps

# DeHashed, Snusbase (paid services)
# More comprehensive breach data
```

**Code snippet sites:**
- Stack Overflow (search for domain/company)
- CodePen, JSFiddle (embedded API keys)
- Ideone, Repl.it (shared code)

### Cloud Storage Exposure

**S3 buckets:**
```bash
# Test bucket accessibility
aws s3 ls s3://bucket-name --no-sign-request

# Check bucket permissions
# Public read = anyone can list and download
# Public write = anyone can upload

# Download bucket contents
aws s3 sync s3://bucket-name ./bucket-name --no-sign-request
```

**Azure blob storage:**
```bash
# Try to list containers
curl "https://account.blob.core.windows.net/?comp=list"

# Try to list blobs in container
curl "https://account.blob.core.windows.net/container?restype=container&comp=list"
```

**Google Cloud Storage:**
```bash
# List bucket
curl "https://storage.googleapis.com/bucket-name/"

# Try to download files
wget https://storage.googleapis.com/bucket-name/file.txt
```

**What to look for in cloud storage:**
- Database backups
- Log files
- Configuration files
- Customer data
- Internal documents
- Credentials and keys
- Source code

## Third-Party and Supply Chain Mapping

Understanding dependencies is critical for supply chain threat modeling.

### Vendor Identification

**JavaScript includes:**
Every third-party script is a vendor relationship:
```bash
# Extract all script sources
curl -s https://example.com | grep -o 'src="[^"]*"' | cut -d'"' -f2

# Common third-parties:
# - Analytics: Google Analytics, Mixpanel, Segment
# - Tag managers: Google Tag Manager, Tealium
# - CDNs: Cloudflare, Fastly, Akamai
# - Chat: Intercom, Drift, Zendesk
# - Payment: Stripe, PayPal, Braintree
# - Advertising: DoubleClick, AdRoll
```

**Privacy policy mining:**
Privacy policies list third-party services:
```bash
# Download and search privacy policy
curl -s https://example.com/privacy | grep -i "third party"

# Look for mentions of:
# - Cloud providers
# - Payment processors
# - Analytics services
# - Email providers
# - CRM systems
```

**API endpoints:**
```bash
# Proxy traffic and watch API calls
# Look for third-party domains

# Common patterns:
# api.service.com
# *.service.io
# service-api.example.com
```

### SaaS Footprint

**Login page discovery:**
Many SaaS tools have predictable login URLs:
```
# Common patterns
https://company-name.service.com
https://service.com/company-name
https://app.service.com (with company-name as login)

# Test for common SaaS:
# - Salesforce: company.my.salesforce.com
# - Slack: company.slack.com
# - Atlassian: company.atlassian.net
# - Okta: company.okta.com
# - Zendesk: company.zendesk.com
# - GitHub: github.com/company
# - GitLab: gitlab.com/company
```

**SSO discovery:**
If company uses SSO, login pages reveal identity provider:
- Okta
- OneLogin
- Azure AD
- Google Workspace
- Ping Identity

**Email service provider:**
MX records already revealed this, but confirm:
- Google Workspace (Gmail)
- Microsoft 365 (Exchange Online)
- Proofpoint (often in front of others)

### API and Webhook Discovery

**API documentation:**
```
# Common paths
/api/v1/
/api/docs
/swagger
/api-docs
/graphql
/v1/
/v2/

# Documentation platforms
# - Swagger UI
# - Redoc
# - GraphQL Playground
# - Postman documentation
```

**Webhook endpoints:**
```bash
# Search code repositories for webhook URLs
grep -r "webhook" .
grep -r "callback" .

# Webhook patterns often reveal third-party services
```

### Supply Chain Dependencies

**Frontend dependencies:**
```bash
# Check for package.json
curl -s https://example.com/package.json

# Likely paths:
# /package.json
# /static/package.json
# /assets/package.json

# JavaScript source maps may reference dependencies
curl -s https://example.com/static/js/main.js.map
```

**Backend dependencies:**
Harder to discover externally, but look for:
- Error messages mentioning libraries
- Default paths for frameworks
- Known vulnerable dependency indicators

**Infrastructure dependencies:**
- Container registry usage (Docker Hub, ECR, GCR)
- Package managers (npm, pip, Maven)
- CI/CD platforms (Jenkins, CircleCI, GitHub Actions)

## Reconnaissance Tools and Automation

### All-in-One Frameworks

**Recon-ng:**
```bash
# Launch recon-ng
recon-ng

# Load workspaces
workspaces create company-name

# Add domain
db insert domains
example.com

# Run modules
modules load recon/domains-hosts/bing_domain_web
run

# Export results
show hosts
```

**Spiderfoot:**
```bash
# Run spiderfoot
sf -s example.com -o output/

# Web interface
spiderfoot -l 127.0.0.1:5001
```

**theHarvester:**
```bash
# Email and subdomain harvesting
theHarvester -d example.com -b all -f output

# Sources: Google, Bing, LinkedIn, etc.
```

### Custom Automation

**Bash scripting example:**
```bash
#!/bin/bash
DOMAIN=$1

echo "[+] Starting reconnaissance for $DOMAIN"

# Subdomain enumeration
echo "[+] Running subfinder..."
subfinder -d $DOMAIN -o subdomains-subfinder.txt

echo "[+] Running amass..."
amass enum -d $DOMAIN -o subdomains-amass.txt

# Combine and dedupe
cat subdomains-*.txt | sort -u > all-subdomains.txt

# Probe for live hosts
echo "[+] Probing for live hosts..."
httpx -l all-subdomains.txt -o live-hosts.txt

# Technology detection
echo "[+] Detecting technologies..."
whatweb -i live-hosts.txt -a 3 --log-json=tech-detection.json

# Screenshots
echo "[+] Taking screenshots..."
gowitness file -f live-hosts.txt

echo "[+] Reconnaissance complete"
```

### Continuous Monitoring

Set up monitoring for changes:
```bash
# Monitor for new subdomains
# Run enumeration daily and diff results
subfinder -d example.com -o today-$(date +%Y%m%d).txt
diff yesterday.txt today.txt

# Monitor certificate transparency
# Use services like certstream or Facebook's CT monitoring

# Monitor for new code repositories
# Use GitHub's watch features or API polling

# Monitor for new IP allocations
# Watch BGP announcements for organization's ASNs
```

## Documentation and Output

### Organizing Findings

**Create structured output:**
```
reconnaissance/
├── domains.txt                 # All discovered domains
├── subdomains.txt             # All discovered subdomains
├── live-hosts.txt             # Responding web services
├── ip-addresses.txt           # All discovered IPs
├── netblocks.txt              # Identified IP ranges
├── technologies.json          # Technology fingerprints
├── employees.csv              # Employee data from LinkedIn
├── repositories.txt           # Code repositories
├── cloud-resources.txt        # S3 buckets, blob storage, etc.
├── api-endpoints.txt          # Discovered API endpoints
├── third-party-services.txt   # Vendor and SaaS tools
└── screenshots/               # Visual documentation
```

**Asset database:**
Create a master spreadsheet or database with:
- Asset identifier
- Asset type (domain, subdomain, IP, repository, etc.)
- Discovery method
- Status (active, inactive, abandoned)
- Technologies detected
- Sensitivity level (to be determined in Chapter 3)
- Notes and observations

### Red Flags and Immediate Findings

Document critical findings immediately:
- Exposed credentials or API keys
- Publicly accessible databases
- Admin panels without authentication
- Vulnerable software versions (actively exploited)
- Subdomain takeover opportunities
- Data breaches containing company information

These need immediate client notification, not just inclusion in the final report.

## Common Pitfalls

**Incomplete enumeration:**
Don't stop at the first tool. Different tools find different results. Combine outputs.

**Not documenting discovery method:**
When you find something interesting, note how you found it. Client needs to understand and potentially monitor.

**Ignoring historical data:**
Old configurations, deprecated systems, and archived content still represent risk.

**Missing cloud resources:**
Cloud storage enumeration is easy to overlook but critical. Test many naming patterns.

**Forgetting about mobile:**
Mobile apps have their own infrastructure, APIs, and potential exposures. Include in reconnaissance.

**Scope creep:**
Stay within authorized boundaries. Don't start testing vulnerabilities during discovery.

## Next Steps

With reconnaissance complete, you have a comprehensive list of assets and context. Chapter 3 covers organizing this data into a structured asset inventory and classifying assets by criticality and data sensitivity.

The raw discovery data is just raw data. The next phase turns it into actionable intelligence.