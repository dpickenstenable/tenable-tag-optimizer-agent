# Tenable Tag Optimizer Agent

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude-Code-blue)](https://claude.ai/code)
[![Tenable](https://img.shields.io/badge/Tenable-Vulnerability%20Management-red)](https://www.tenable.com/)

> **AI-powered tag recommendation engine for Tenable that analyzes your asset landscape and creates intelligent, environment-specific tag taxonomies automatically.**

## 🎯 Overview

The **Tenable Tag Optimizer Agent** analyzes your Tenable environment, identifies natural asset groupings, and intelligently recommends tag categories and values based on your actual infrastructure. It then automates tag creation to establish a clean, consistent taxonomy that supports compliance scoping, risk prioritization, and operational workflows.

This agent helps security and IT teams:
- 🏷️ **Automate tag creation** - No more manual clicking through the Tenable UI
- 📊 **Environment-specific taxonomy** - Tags that match your actual assets, not generic templates
- 🎯 **Smart pattern detection** - Identifies natural groupings from hostnames, IPs, software, and more
- ✅ **Clean naming conventions** - Consistent, understandable category and value naming
- 🔄 **Quarterly optimization** - Re-run as your environment evolves to add new tags
- 💼 **Business-aligned** - Tags that enable real workflows (patching, compliance, risk management)

### Key Features

✅ **Pattern Recognition** - Analyzes OS, network locations, hostnames, software, criticality  
✅ **Existing Tag Analysis** - Reviews current tags to avoid duplicates and identify gaps  
✅ **8 Core Categories** - Environment, OS, Function, Location, Criticality, Compliance, Patch Windows, Cloud Provider  
✅ **Intelligent Value Generation** - Only suggests values that apply to actual assets (no theoretical tags)  
✅ **Business Value Mapping** - Explains how each tag enables specific workflows  
✅ **Dual Authentication** - Tenable MCP Server or Direct API  
✅ **Bulk Application** - Apply tags to hundreds of assets automatically  
✅ **Dynamic Tag Support** - Can create dynamic tags that auto-update based on filters

---

## 📋 Table of Contents

- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [How It Works](#-how-it-works)
- [Tag Categories](#-tag-categories)
- [Usage Examples](#-usage-examples)
- [Best Practices](#-best-practices)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🚀 Quick Start

### Installation

See **[INSTALL.md](INSTALL.md)** for complete installation and usage instructions.

**Quick install:**
```bash
cd ~/.claude/agents
git clone https://github.com/dpickenstenable/tenable-tag-optimizer-agent.git
cp tenable-tag-optimizer-agent/agent.md tenable-tag-optimizer.md
```

### How to Use

**You interact with this agent using natural language in Claude Code** - no coding required!

Open Claude Code and simply say:
```
Run the Tenable Tag Optimizer
```

The agent will analyze your assets and recommend a clean tag taxonomy.

**Behind the scenes**, Claude Code executes the agent. You don't need to write JavaScript `Agent()` calls or technical commands - just describe what you want in plain English.

---

## 📖 Full Documentation

- **[INSTALL.md](INSTALL.md)** - Complete installation and usage guide
  - What you need
  - Step-by-step setup
  - Authentication options
  - Natural language examples
  - Complete walkthrough
  - 10 advanced usage scenarios
  - Troubleshooting
  - Security & privacy

- **[README.md](README.md)** - This file (overview and technical details)

---

## 🔍 How It Works

### Phase 1: Asset Discovery & Pattern Recognition

The agent retrieves your asset inventory and analyzes:

**Operating System Patterns:**
- OS families (Windows, Linux, macOS, Network Devices)
- Version distributions
- End-of-life status

**Network Patterns:**
- Subnet groupings (10.1.x.x = Production, 10.2.x.x = Development)
- VLAN segments
- DMZ vs Internal networks

**Hostname Patterns:**
- Environment prefixes: `prod-`, `dev-`, `qa-`, `staging-`
- Location indicators: `nyc-`, `lax-`, `lon-`
- Function indicators: `web-`, `db-`, `dc-`
- Business unit codes: `fin-`, `hr-`, `eng-`

**Application/Software Patterns:**
- Database servers (SQL Server, PostgreSQL, MySQL)
- Web servers (IIS, Apache, Nginx)
- Virtualization (VMware, Hyper-V)
- Cloud services (AWS, Azure, GCP)

**Criticality Patterns:**
- High AES scores (800+)
- Domain Controllers, Exchange Servers
- Business-critical functions

### Phase 2: Intelligent Recommendation

Based on detected patterns, the agent recommends tag categories following these principles:

**✅ Data-Driven** - Only suggests tags that apply to actual assets  
**✅ Coverage Threshold** - Each category must apply to 20+ assets  
**✅ Value Minimum** - Each value must apply to 5+ assets  
**✅ Clean Naming** - Consistent conventions (Capitalized categories, lowercase-hyphenated values)  
**✅ Business Aligned** - Tags that enable real workflows

**Recommended Core Categories** (when data supports them):

1. **Environment** - Production, development, QA, staging
2. **Operating System** - Windows, Linux, macOS, network devices
3. **Function** - Web server, database, domain controller, application server
4. **Location** - Data center locations, cloud regions
5. **Business Unit** - Finance, HR, engineering, sales
6. **Criticality** - Critical, high, medium, low
7. **Compliance Scope** - PCI-DSS, HIPAA, SOX, GDPR
8. **Patch Window** - Maintenance window assignments
9. **Cloud Provider** - AWS, Azure, GCP, on-premises
10. **Lifecycle Status** - Supported, end-of-life, extended support

### Phase 3: Tag Creation & Application

After your approval:

1. **Creates tag categories and values** via Tenable API
2. **Tracks progress** with real-time updates
3. **Handles errors gracefully** (duplicates skipped, rate limits respected)
4. **Optionally applies tags** to assets based on detected patterns
5. **Generates summary report** with coverage metrics

---

## 🏷️ Tag Categories

### Core Categories Explained

#### Environment
**Purpose:** Distinguish production from non-production assets

**Business Value:**
- Exclude dev/qa from compliance audits (reduce scope)
- Prioritize production vulnerabilities
- Test patches in dev before production
- Cost allocation (production vs. non-production)

**Example Values:** `production`, `development`, `qa`, `staging`

---

#### Function
**Purpose:** Group assets by business or technical function

**Business Value:**
- Targeted patching campaigns (patch all databases together)
- Exposure card creation (monitor crown jewels)
- Incident response (quickly identify affected functions)
- Configuration management (apply function-specific policies)

**Example Values:** `web-server`, `database`, `domain-controller`, `application-server`

---

#### Patch Window
**Purpose:** Assign assets to maintenance windows

**Business Value:**
- Automated patch scheduling
- Avoid patching production during business hours
- Change management tracking
- SLA compliance (critical: 48h, high: 7d)

**Example Values:** `window-1-sunday-2am`, `window-2-tuesday-10pm`, `critical-only`, `manual-approval`

---

#### Compliance Scope
**Purpose:** Identify assets in regulatory scope

**Business Value:**
- Focused compliance audits
- Automated compliance reporting
- Risk-based remediation (in-scope first)
- Evidence collection for auditors

**Example Values:** `pci-in-scope`, `hipaa-phi`, `sox-financial`, `gdpr-personal-data`, `out-of-scope`

---

## 📖 Usage Examples

### Example 1: First-Time Setup

```
User: "Run the Tenable Tag Optimizer"

Agent:
✓ Retrieves 1,234 assets
✓ Detects patterns (hostnames, IP ranges, software)
✓ Recommends 6 tag categories with 28 total values
✓ User approves recommendations
✓ Creates all 28 tags
✓ Applies tags to 1,180 assets (95.6% coverage)
✓ Generates summary report
```

**Result:** Clean tag taxonomy covering 95%+ of assets, ready for compliance scoping and risk prioritization.

---

### Example 2: Adding Compliance Tags

```
User: "We need PCI DSS scoping tags. Can you add those?"

Agent:
✓ Analyzes existing tags (finds no compliance tags)
✓ Asks: "Which assets are in the PCI DSS cardholder data environment?"
✓ User provides: "Anything on the 10.5.x.x network or with 'pci' in hostname"
✓ Creates:
  - Compliance Scope:pci-in-scope (125 assets)
  - Compliance Scope:pci-out-of-scope (1,109 assets)
✓ Applies tags
✓ Suggests creating Exposure Card for PCI-in-scope assets
```

**Result:** Clear PCI DSS boundary established, enabling focused compliance audits and reducing audit scope by 90%.

---

### Example 3: Quarterly Tag Review

```
User: "It's been 3 months. Run tag optimization again to see what's changed."

Agent:
✓ Retrieves current asset inventory (now 1,456 assets - grew by 222)
✓ Analyzes existing tags (finds 6 categories, 28 values from previous run)
✓ Detects new patterns:
  - 45 new Kubernetes pods (no existing "Container Platform" tag)
  - 30 new GCP assets (no "Cloud Provider:gcp" tag)
✓ Recommends:
  - Add "Container Platform" category with "kubernetes", "docker", "none" values
  - Add "gcp" value to existing "Cloud Provider" category
✓ User approves
✓ Creates 4 new tags
✓ Applies to 75 assets
✓ Reports: "222 new assets discovered. 75 tagged with new categories. 147 tagged with existing categories."
```

**Result:** Tag taxonomy evolves with your infrastructure. New asset types automatically get appropriate tags.

---

## 🎯 Best Practices

### Tag Naming Conventions

**Category Naming:**
- ✅ DO: "Environment", "Operating System", "Business Unit"
- ❌ DON'T: "Env", "OS", "BU" (too abbreviated)
- ✅ DO: Singular form ("Environment" not "Environments")

**Value Naming:**
- ✅ DO: "production", "domain-controller", "pci-dss"
- ❌ DON'T: "Production", "DomainController", "PCI DSS" (mixed case/spaces)
- ✅ DO: Hyphenate multi-word values ("web-server" not "web_server")

### Avoid Tag Sprawl

**Warning signs:**
- >20 categories (too many to manage)
- Values with <5 assets (too granular)
- Overlapping categories ("Environment" and "Deployment Stage")
- Too many values per category (>15 becomes unmanageable)

**Keep it simple:**
- Start with 5-8 core categories
- Add more categories later as needs evolve
- Focus on tags that enable **workflows** (patching, compliance, risk prioritization)

### Dynamic Tags

For tags that should auto-update, use **dynamic tags** with filters:

**Example:** Operating System tags
```
Tag: Operating System:windows
Filter: operating_system contains 'Windows'
Result: Automatically includes new Windows assets as they're discovered
```

**Benefits:**
- Always up-to-date
- No manual tagging required
- Reduced maintenance overhead

### Tag Governance

After creating tags, implement governance:

1. **Access Control** - Restrict who can modify tags
2. **Change Log** - Document why tags were created
3. **Review Cadence** - Quarterly tag audit
4. **Naming Standards** - Document convention for future additions
5. **Training** - Ensure team understands tag meanings

### Quarterly Review Schedule

**Recommended cadence:**
- **Quarterly** - Full tag optimization review
- **As needed** - When major infrastructure changes occur
- **Annually** - Tag governance policy review

---

## 🐛 Troubleshooting

### Permission Errors

**Error:** `403 Forbidden - Insufficient permissions to create tags`

**Solution:** You need the **"Can Manage"** permission on Tags.

1. Ask a Tenable administrator to navigate to: **Settings → Access Control → Groups**
2. Edit your user group
3. Under **Tags**, enable **"Can Manage"**
4. Save changes and try again

---

### Duplicate Tag Errors

**Error:** `409 Conflict - Tag already exists`

**What happens:** Agent skips and continues with next tag

**Solution:** No action needed - duplicates are handled automatically

---

### Rate Limit Errors

**Error:** `429 Too Many Requests`

**What happens:** Agent implements exponential backoff (5s → 10s → 30s)

**Solution:** Wait for automatic retry. If persistent, contact Tenable support.

---

### No Patterns Detected

**Error:** "I couldn't detect enough patterns to recommend tags"

**Causes:**
- Asset count too low (<100 assets)
- Inconsistent naming conventions
- Already comprehensive tag coverage

**Solution:**
- For small environments: Manually create tags
- For inconsistent naming: Standardize hostname patterns first
- For complete coverage: Consider this a success!

---

## 📊 Output Examples

### Summary Report

```
## Tag Optimization Complete!

Environment: Tenable VM
Completion Date: 2026-06-23

Tags Created: 6 categories, 28 values

By Category:
- Environment: 4 values (production, development, qa, staging)
- Operating System: 4 values (windows, linux, macos, network-device)
- Function: 6 values (web-server, database, domain-controller, etc.)
- Location: 5 values (us-east, us-west, emea, apac, cloud)
- Criticality: 4 values (critical, high, medium, low)
- Patch Window: 5 values (window-1-sunday-2am, etc.)

Assets Tagged: 1,180 of 1,234 (95.6%)
Untagged Assets: 54 (4.4%) - didn't fit detected patterns

Next Steps:
✅ Create Exposure Cards using new tags
✅ Configure access control for tags
✅ Update scan policies with tag filters
✅ Build dashboards filtered by tags
✅ Set remediation SLAs based on tags
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

### Reporting Issues

- Check existing issues before creating a new one
- Provide asset count and existing tag count
- Include error messages if applicable

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Test with real Tenable environments
4. Commit with clear messages
5. Push and open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Claude Code** by Anthropic - AI-powered development environment
- **Tenable** - Vulnerability Management and Exposure Management platform
- **Tenable MCP Server** - Model Context Protocol integration for Tenable API

---

## 📞 Support

- **Issues:** [GitHub Issues](https://github.com/dpickenstenable/tenable-tag-optimizer-agent/issues)
- **Tenable Support:** [Tenable Community](https://community.tenable.com/)
- **Claude Code:** [claude.ai/code](https://claude.ai/code)

---

## 🗺️ Roadmap

- [ ] Multi-tenant support (analyze multiple Tenable instances)
- [ ] Tag usage analytics (which tags are most valuable)
- [ ] Integration with Exposure Cards (auto-create cards from tags)
- [ ] Tag health monitoring (orphaned tags, coverage gaps)
- [ ] Export tag mapping to CSV/Excel
- [ ] Slack/Email notifications for tag recommendations
- [ ] Custom tag naming convention templates

---

## 🔗 Related Projects

- [Tenable MCP Server](https://github.com/tenable/mcp-server-tenable) - Model Context Protocol server for Tenable API
- [Tenable Health Check Agent](https://github.com/d-zito/tenable-healthcheck-agent) - Infrastructure health monitoring
- [Tenable Audit Log Review Agent](https://github.com/dpickenstenable/tenable-audit-log-review-agent) - Daily audit log analysis

---

**Made with ❤️ for the Tenable community**

If this agent helps your asset organization, please ⭐ star the repository!
