# DNSGuard

> **Your domains. Your reputation. Your shield.** Continuous DNS health, DMARC
> compliance, and domain intelligence for multi-tenant Microsoft environments.
> Guard your perimeter before the auditor asks.

[![License: MIT](https://img.shields.io/badge/License-MIT-success.svg)](LICENSE)
[![Part of TenantFleet](https://img.shields.io/badge/TenantFleet-Spirit-2e7d32)](https://t-granlund.github.io/tenantfleet/)
[![Live demo](https://img.shields.io/badge/demo-live-1f6feb)](https://t-granlund.github.io/dnsguard/)
[![Python](https://img.shields.io/badge/python-3.11%2B-3776ab)](#architecture)

DNSGuard is the **Spirit** pillar of the
[TenantFleet](https://t-granlund.github.io/tenantfleet/) ecosystem — the domain
security and perimeter-intelligence layer. It continuously watches the DNS and
email-authentication posture of every domain across every tenant, so a lapsed
DMARC policy or an expiring domain surfaces before it becomes an incident.

---

## Why it exists

In a multi-brand or PE portfolio, nobody has a single view of domain health.
SPF/DKIM/DMARC drift silently, domains quietly approach expiry, and the first
time anyone notices is during an audit — or a phishing campaign. DNSGuard makes
perimeter posture **continuous and measurable**.

## What it delivers

| Capability | What it does |
| --- | --- |
| **DMARC monitoring** | Aggregate and forensic reports parsed daily with trend analysis and compliance scoring per domain. |
| **DNS health** | Real-time checks for A, AAAA, MX, NS, TXT, and SOA records with propagation monitoring. |
| **Domain expiry** | WHOIS and registrar monitoring with configurable alert thresholds before expiration. |
| **Email security scoring** | Composite score based on SPF, DKIM, DMARC, MTA-STS, and TLS-RPT posture. |
| **Threat intelligence** | Correlation against known phishing infrastructure and suspicious registrar activity. |
| **PE reporting** | Portfolio-level dashboards for private-equity domain posture and compliance readiness. |

**At a glance:** 5 protocols monitored · &lt; 0.1% false-positive rate · ∞ domain
scale · &lt; 60s alert latency.

## Install

```bash
git clone https://github.com/t-granlund/dnsguard.git
cd dnsguard
pip install -r requirements.txt   # requests, dnspython, etc.
```

Point DNSGuard at a tenant and a brand; it discovers and audits the brand's
domains.

## Usage

```python
from dnsguard import DNSChecker, DMARCAnalyzer

# Initialize checker for your tenant
checker = DNSChecker(tenant_id="TENANT_ID")

# Audit all domains for a brand
report = checker.audit_brand_domains(brand="acme-corp")

# DMARC compliance check
dmarc = DMARCAnalyzer(report.records)
assert dmarc.policy == "quarantine"
assert dmarc.pct == 100
```

The same audit runs unattended on a schedule, emitting per-domain compliance
scores and alerts.

## Architecture

```
brand domains ──► DNSChecker ──► record snapshot (A/AAAA/MX/NS/TXT/SOA)
                       │
                       ├──► DMARCAnalyzer  (aggregate + forensic reports)
                       ├──► email-security scorer (SPF/DKIM/DMARC/MTA-STS/TLS-RPT)
                       ├──► WHOIS / expiry monitor
                       └──► threat-intel correlation
                                   │
                                   ▼
                    per-domain compliance score + alerts + PE dashboards
```

- **DNSChecker** resolves and snapshots every record type, with propagation
  awareness so transient states don't trigger false alerts.
- **DMARCAnalyzer** parses aggregate and forensic XML, scoring policy strength
  (`none` → `quarantine` → `reject`) and coverage (`pct`).
- **Email-security scorer** rolls SPF, DKIM, DMARC, MTA-STS, and TLS-RPT into a
  single composite posture score.
- **Reporting** aggregates domain scores into portfolio-level views for PE and
  compliance stakeholders.

## Security

- **Read-only** — DNSGuard observes; it does not mutate DNS.
- **No secrets required** for public DNS/WHOIS checks.
- **Auditable** — every score traces back to the records that produced it.

## Part of the TenantFleet ecosystem

| Repo | Pillar | Focus |
| --- | --- | --- |
| [TenantFleet](https://t-granlund.github.io/tenantfleet/) | — | Root governance framework |
| [HubForge](https://t-granlund.github.io/hubforge/) | Mind | Azure SWA + Entra deployment templates |
| [EntraGroups](https://t-granlund.github.io/entragroups/) | Body | Group lifecycle + persona RBAC |
| [TenantForge](https://t-granlund.github.io/tenantforge/) | Mind | Terraform tenant provisioning |
| **DNSGuard** | Spirit | Domain + DMARC intelligence |
| [RampGuard](https://t-granlund.github.io/rampguard/) | Mind | Finance compliance + spend |
| [SharePointAgent](https://t-granlund.github.io/sharepoint-agent/) | Body | SharePoint indexing + Teams |

See the full ecosystem on the portfolio:
[tylergranlund.com/work#ecosystem](https://tylergranlund.com/work#ecosystem).

## License

MIT — see [LICENSE](LICENSE). Fork it, deploy it, make it yours.
