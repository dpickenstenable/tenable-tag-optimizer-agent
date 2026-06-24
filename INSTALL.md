# Installation & Usage Guide

This guide explains how to install and run the Tenable Tag Optimizer agent in Claude Code.

## What You Need

1. **Claude Code** - The CLI, desktop app, or web interface at claude.ai/code
2. **Tenable VM or Tenable One** - Active instance with discovered assets
3. **API Access** - Either Tenable MCP Server OR Tenable API keys
4. **Permissions** - "Can Manage" permission on Tags in Tenable (required to create tags)

## What This Agent Does

The Tenable Tag Optimizer is your AI-powered asset organization specialist. It analyzes your Tenable environment to identify natural asset groupings (by OS, function, location, environment), then intelligently recommends and creates a clean tag taxonomy that matches your actual infrastructure.

**Why this matters:**
- **Manual tagging is tedious** - Clicking through the UI to create dozens of tags takes hours
- **Generic templates don't fit** - Your environment is unique; you need tags that match your actual assets
- **Consistency is hard** - Without guidance, tags become messy (Env vs Environment, prod vs Production)
- **Tags enable workflows** - Proper tags support compliance scoping, risk prioritization, and patch management

**What you get:**
- Intelligent tag recommendations based on your actual asset data
- Automated tag creation (no manual clicking)
- Clean naming conventions (consistent, understandable)
- Business-aligned categories (Environment, Function, Criticality, etc.)
- High coverage (95%+ of assets tagged)

## Installation

### Option 1: Install from Repository (Recommended)

```bash
# Clone the repository
cd ~/.claude/agents
git clone https://github.com/dpickenstenable/tenable-tag-optimizer-agent.git

# Copy the agent definition
cp tenable-tag-optimizer-agent/agent.md ~/.claude/agents/tenable-tag-optimizer.md

# Verify installation
ls -la ~/.claude/agents/tenable-tag-optimizer.md
```

### Option 2: Manual Installation

1. Download the `agent.md` file from this repository
2. Save it to your Claude Code agents directory:
   - **macOS/Linux:** `~/.claude/agents/tenable-tag-optimizer.md`
   - **Windows:** `%USERPROFILE%\.claude\agents\tenable-tag-optimizer.md`

## Pre-Installation Setup

### Step 1: Verify Tag Permissions

You need "Can Manage" permission on Tags to create new tags.

**Check your permissions:**
1. Log into Tenable (cloud.tenable.com or your on-prem URL)
2. Navigate to **Settings → Access Control → Groups**
3. Find your user group
4. Under **Tags**, verify **"Can Manage"** is enabled

**If you don't have this permission:**
- Ask your Tenable administrator to grant it
- Without this, the agent can recommend tags but cannot create them

### Step 2: Set Up Authentication

Choose one of two authentication methods:

#### Method A: Tenable MCP Server (Recommended)

If you have the Tenable MCP Server configured, the agent will automatically use it.

**Benefits:**
- Automatic authentication
- Rate limiting protection
- No credential management needed

**Setup:** Follow the [Tenable MCP Server installation guide](https://github.com/tenable/mcp-server-tenable)

#### Method B: Direct API Authentication

If MCP is unavailable, set environment variables:

```bash
# macOS/Linux
export TENABLE_ACCESS_KEY="your_access_key_here"
export TENABLE_SECRET_KEY="your_secret_key_here"

# Windows PowerShell
$env:TENABLE_ACCESS_KEY="your_access_key_here"
$env:TENABLE_SECRET_KEY="your_secret_key_here"
```

**Generating API Keys:**
1. Log into Tenable
2. Navigate to **Settings → My Account → API Keys**
3. Click **Generate**
4. Copy both Access Key and Secret Key
5. **Required Permissions:** Can View Assets, Can Manage Tags

## How to Run the Agent

The agent runs inside Claude Code conversations. You interact with it using natural language.

### Step 1: Start Claude Code

Open Claude Code in your preferred interface:
- **CLI:** Run `claude` in your terminal
- **Desktop:** Launch the Claude Code app
- **Web:** Visit claude.ai/code

### Step 2: Run the Agent with Natural Language

In the Claude Code conversation, simply type:

```
Run the Tenable Tag Optimizer
```

Or be more specific:

```
Use the Tenable Tag Optimizer agent to analyze my assets and
recommend a clean tag taxonomy
```

**Behind the scenes**, Claude Code will execute the agent. You don't need to write any JavaScript or technical commands.

### Using Opus Model for More Thorough Analysis

By default, agents run with the Sonnet model. For more comprehensive, thorough analysis, you can upgrade to the **Opus model with high effort**.

**When to use Opus:**
- Large asset inventories (1000+ assets), establishing new tagging taxonomy, quarterly tag audits, migrating from legacy tags
- Need deeper analysis and more detailed recommendations
- Want exhaustive reports with step-by-step details
- Preparing for audits requiring comprehensive documentation
- First-time analysis of a new environment

**How to use Opus:**

In your Claude Code conversation, specify the model before invoking the agent:

```
Switch to Opus model

Then run the Tenable Tag Optimizer with comprehensive tag taxonomy analysis
```

**Or use the direct command:**

```
/model opus

Run the Tenable Tag Optimizer with comprehensive tag taxonomy analysis
```

**What changes with Opus + high effort:**
- ✅ More detailed analysis for each finding
- ✅ Deeper context about why issues matter
- ✅ More comprehensive recommendations
- ✅ Additional cross-references between related items
- ✅ More thorough validation steps
- ✅ Longer, more detailed reports (expect 2-3x length)

**Trade-offs:**
- ⏱️ Takes longer to complete (2-5x slower than Sonnet)
- 💰 Uses more tokens (higher cost)
- 📄 Generates longer reports (more to read)

**Recommendation:** Start with Sonnet for routine analysis, use Opus for quarterly deep-dives or audit preparation.

### What Happens Next

1. **Agent starts** - Claude Code spawns the Tenable Tag Optimizer
2. **Asset discovery** - Agent retrieves all assets from Tenable
3. **Pattern recognition** - Analyzes hostnames, IPs, OS, software
4. **Tag recommendations** - Suggests 5-8 categories with specific values
5. **Interactive review** - Presents recommendations for your approval
6. **Tag creation** - Creates all approved tags automatically
7. **Tag application** - Optionally applies tags to assets
8. **Summary report** - Shows coverage and next steps

## Complete Example Walkthrough

### Scenario: First-time tag setup for a new Tenable deployment

**Your situation:**
- Tenable deployed 1 month ago
- 1,234 assets discovered
- No organized tagging strategy
- Need to scope compliance audits and prioritize vulnerabilities
- Manual tagging would take days

**Step 1: Start Claude Code**
```bash
claude
```

**Step 2: Run the agent**
```
Run the Tenable Tag Optimizer to create a clean tag taxonomy for my environment
```

**Step 3: Agent analyzes your environment**
```
Analyzing Tenable environment...

✓ Retrieved 1,234 assets
✓ Analyzing existing tags (found 3 categories, 8 values)
✓ Detecting patterns in hostnames
✓ Analyzing network ranges
✓ Identifying OS distributions
✓ Detecting installed software

Analysis complete!
```

**Step 4: Review recommendations**

The agent presents a comprehensive report:

```
## Tag Optimizer - Recommendations

I analyzed 1,234 assets and recommend creating 6 new tag categories
with 28 total values. These tags will organize 95.6% of your assets.

### Recommended Categories:

1. Environment (NEW) - 4 values
   - production (450 assets) - Hostnames with prod-, IPs in 10.1.x.x
   - development (120 assets) - Hostnames with dev-, IPs in 10.2.x.x
   - qa (80 assets) - Hostnames with qa-
   - staging (30 assets) - Hostnames with staging-

2. Operating System (NEW) - 4 values
   - windows (850 assets)
   - linux (320 assets)
   - macos (15 assets)
   - network-device (25 assets)

3. Function (NEW) - 6 values
   - web-server (95 assets) - IIS, Apache, Nginx detected
   - database (120 assets) - SQL Server, PostgreSQL, MySQL
   - domain-controller (5 assets) - dc- hostnames, Active Directory
   - application-server (200 assets)
   - file-server (45 assets)
   - load-balancer (12 assets)

[... more categories ...]

Total Coverage: 1,180 of 1,234 assets (95.6%)
```

**Step 5: Approve recommendations**

The agent asks:
```
What would you like to do?
1. Approve as-is - Create all recommended tags
2. Adjust - Change category/value names, add/remove categories
3. Preview - Show which assets will get which tags
4. Cancel - Don't create any tags
```

**Choose Option 1: Approve**
```
Approve as-is
```

**Step 6: Agent creates tags**
```
Creating tags...

✓ Created Environment:production
✓ Created Environment:development
✓ Created Environment:qa
✓ Created Environment:staging
✓ Created Operating System:windows
✓ Created Operating System:linux
✓ Created Operating System:macos
✓ Created Operating System:network-device

Progress: [================>] 28/28 tags created

All tags created successfully!
```

**Step 7: Optionally apply tags**

The agent asks:
```
Tags created! Would you like me to apply these tags to assets
based on detected patterns? This will tag approximately 1,180 assets.

Options:
1. Apply now - Automatically tag all assets
2. Manual application - I'll give you guidance
3. Skip - Tags created but not applied yet
```

**Choose Option 1: Apply now**
```
Apply now
```

**Step 8: Agent applies tags**
```
Applying tags to assets...

Environment:production
  ✓ Batch 1/5: 100 assets tagged
  ✓ Batch 2/5: 100 assets tagged
  ...

Operating System:windows
  ✓ Batch 1/9: 100 assets tagged
  ...

Total: 1,180 assets tagged (95.6% coverage)

Tag optimization complete!
```

## Advanced Usage Examples

### Example 1: Adding Compliance Tags

```
I need PCI DSS scoping tags. Assets on the 10.5.x.x network and
anything with 'pci' in the hostname are in scope. Can you add those tags?
```

The agent will:
- Create "Compliance Scope" category
- Add "pci-in-scope" and "pci-out-of-scope" values
- Apply tags based on your criteria (network range + hostname pattern)
- Report: "125 assets tagged as pci-in-scope, 1,109 as pci-out-of-scope"

### Example 2: Quarterly Tag Review

```
Run the Tenable Tag Optimizer again. It's been 3 months since we last
optimized tags. See what's changed.
```

The agent will:
- Retrieve current asset inventory (now 1,456 assets - grew by 222)
- Analyze existing tags
- Detect new patterns (45 Kubernetes pods, 30 GCP assets)
- Recommend new categories/values for new asset types
- Report: "222 new assets discovered. Recommend adding 'Container Platform' category."

### Example 3: Location-Based Tags

```
Create location tags based on our data centers:
- 10.10.x.x = New York
- 10.20.x.x = London
- 10.30.x.x = Singapore
- Everything else = Cloud
```

The agent will:
- Create "Location" category
- Add values: new-york, london, singapore, cloud
- Apply tags based on IP ranges
- Report coverage per location

### Example 4: Criticality Tags Based on AES

```
Create criticality tags based on Asset Exposure Score:
- Critical: AES > 800
- High: AES 600-799
- Medium: AES 400-599
- Low: AES < 400
```

The agent will:
- Create "Criticality" category with 4 values
- Analyze AES distribution
- Apply tags based on scores
- Suggest creating dynamic tags that auto-update as AES changes

### Example 5: Patch Window Assignments

```
Create patch window tags for our maintenance schedules:
- Window 1: Sunday 2 AM
- Window 2: Tuesday 10 PM
- Window 3: Thursday 11 PM
- Critical-Only: Emergency patching only
```

The agent will:
- Create "Patch Window" category
- Add values for each window
- Ask how to assign assets (by environment, function, or manual)
- Provide guidance on using tags to automate patch scheduling

### Example 6: Business Unit Tags

```
Create business unit tags. We have Finance, HR, Engineering, Sales, and Operations.
Finance systems have 'fin-' in hostnames. HR is on 10.40.x.x network.
```

The agent will:
- Create "Business Unit" category
- Add values for each unit
- Apply tags based on hostname and network patterns
- Ask how to tag assets that don't match patterns

### Example 7: Cloud Provider Tags

```
Tag assets by cloud provider:
- AWS (detect via AWS metadata or 'aws' in hostname)
- Azure (detect via Azure metadata or 'az-' prefix)
- GCP (detect via GCP metadata or 'gcp-' prefix)
- On-Premises (everything else)
```

The agent will:
- Create "Cloud Provider" category
- Analyze asset metadata and hostnames
- Report distribution: "AWS: 450, Azure: 200, GCP: 30, On-Prem: 554"

### Example 8: Consolidating Existing Tags

```
We have inconsistent tags: 'Env', 'Environment', 'Deployment',
'Prod', 'Production', etc. Can you consolidate these into
a clean 'Environment' category?
```

The agent will:
- Analyze existing tags
- Propose consolidation mapping
- Recommend which tags to keep, which to deprecate
- Provide migration plan

### Example 9: EOL Lifecycle Tags

```
Create lifecycle status tags based on OS versions:
- Supported: Currently supported by vendor
- Extended-Support: Paid extended support
- End-of-Life: No vendor support
```

The agent will:
- Analyze OS inventory
- Check against EOL databases (Windows 7, Server 2008, etc.)
- Create tags automatically
- Report EOL assets requiring attention

### Example 10: Export Tag Mapping

```
Generate a CSV export showing which tags are applied to each asset
```

The agent will:
- Query all assets and their tags
- Generate CSV with columns: Hostname, IP, AES, Environment, OS, Function, etc.
- Save as: `Tenable-Tag-Mapping-2026-06-23.csv`
- Useful for auditing or importing into other tools

## Understanding the Output

### Tag Naming Conventions

The agent follows these standards:

**Categories:**
- Capitalized (e.g., "Environment", "Operating System")
- Singular form ("Environment" not "Environments")
- Descriptive, not abbreviated ("Operating System" not "OS")

**Values:**
- Lowercase (e.g., "production", "windows")
- Hyphenated for multi-word (e.g., "domain-controller", "web-server")
- No spaces (use hyphens)
- Consistent across categories

### Coverage Metrics

**Good coverage:** 90-100% of assets tagged
**Acceptable coverage:** 80-90% of assets tagged
**Needs work:** <80% of assets tagged

**Why some assets aren't tagged:**
- Don't fit detected patterns (e.g., hostname doesn't match conventions)
- Missing metadata (no OS detected, no hostname)
- Edge cases (one-off systems)

**Solution:** Manually tag remaining assets or create catch-all values.

### Dynamic vs. Static Tags

**Static tags:** Manually applied, don't change automatically
**Dynamic tags:** Auto-update based on filters (e.g., "OS contains Windows")

The agent can recommend dynamic tags for:
- Operating System (auto-include new Windows assets)
- Criticality (auto-update based on AES thresholds)
- Lifecycle Status (auto-detect EOL software)

**When to use dynamic:**
- Tags that should auto-update (OS, software)
- Large, frequently changing asset groups
- Reduces manual maintenance

**When to use static:**
- Business context tags (environment, business unit)
- Compliance scopes (manually defined boundaries)
- More control over what gets tagged

## Troubleshooting

### Issue: "Permission denied - Can't create tags"

**Problem:** You lack "Can Manage" permission on Tags.

**Solution:**
1. Ask your Tenable administrator to navigate to: **Settings → Access Control → Groups**
2. Edit your user group
3. Under **Tags**, enable **"Can Manage"**
4. Save changes and retry

### Issue: "Duplicate tag already exists"

**Problem:** A tag you're trying to create already exists.

**What happens:** Agent skips it and continues with next tag.

**No action needed:** This is normal and handled automatically.

### Issue: "No patterns detected"

**Problem:** Agent couldn't find enough consistent patterns to recommend tags.

**Causes:**
- Asset count too low (<100 assets)
- Inconsistent naming conventions
- Already comprehensive tag coverage

**Solution:**
- **For small environments:** Manually create tags (agent works best with 100+ assets)
- **For inconsistent naming:** Standardize hostname patterns first
- **For complete coverage:** Great! Consider this a success.

### Issue: "Rate limit exceeded"

**Problem:** Too many API calls in short time.

**What happens:** Agent implements exponential backoff (wait and retry).

**Solution:** Wait for automatic retry. Usually resolves in 30-60 seconds.

### Issue: "Tags created but not showing in Tenable UI"

**Problem:** Browser cache or UI sync issue.

**Solution:**
1. Refresh Tenable UI (hard refresh: Cmd+Shift+R or Ctrl+Shift+R)
2. Navigate away and back to Tags page
3. Log out and log back in
4. Verify via API: `curl -H "X-ApiKeys: ..." https://cloud.tenable.com/tags/values`

### Issue: "Tag application failed for some assets"

**Problem:** Assets may have been deleted or API timed out.

**Solution:**
- Agent will report which assets failed
- Review error messages
- Manually tag failed assets in Tenable UI
- Re-run tag application for specific category

## Security & Privacy

### Data Access

The agent reads:
- Asset inventory (hostnames, IPs, OS, software)
- Existing tags
- Network information

It does **NOT**:
- Access vulnerability data
- Read credentials
- Modify scan configurations
- Delete assets

### Tag Creation

The agent can:
- Create new tag categories
- Create new tag values
- Apply tags to assets

It will **NOT**:
- Delete existing tags
- Modify existing tag values
- Remove tags from assets

All operations are additive and reversible.

### API Key Security

**Best practices:**
- Use environment variables, not hardcoded keys
- Rotate API keys quarterly
- Use MCP Server when possible (avoids exposing keys)
- Audit tag changes in Tenable audit logs

### Report Security

Generated reports may contain:
- Asset hostnames
- IP addresses
- Network topology

**Protect reports:**
- Store in secure locations
- Don't commit to public repositories
- Redact sensitive data if sharing externally

## Tips for Best Results

### DO:

✅ **Start simple** - Begin with 5-8 core categories, expand later

✅ **Use consistent naming** - Follow the agent's conventions

✅ **Provide context** - "Finance systems have 'fin-' in hostname"

✅ **Run quarterly** - Re-optimize as your environment grows

✅ **Document tag meanings** - Maintain a wiki explaining each category

✅ **Train your team** - Ensure everyone understands tag usage

✅ **Avoid tag sprawl** - Don't create categories without clear business value

✅ **Review existing tags first** - Run agent to see what you already have

### DON'T:

❌ **Don't create too many categories** - >20 categories becomes unmanageable

❌ **Don't create values with <5 assets** - Too granular, not useful

❌ **Don't use mixed naming** - Pick one convention and stick to it

❌ **Don't forget business value** - Every tag should enable a workflow

❌ **Don't skip governance** - Restrict who can modify tags

❌ **Don't try to tag everything** - 80-90% coverage is good enough

❌ **Don't try to run the JavaScript Agent() code yourself** - Let Claude Code handle it

## Next Steps After Running

1. **Verify tags in Tenable UI** - Navigate to Settings → Tags
2. **Create Exposure Cards** - Use new tags to build focused views
3. **Configure access control** - Restrict tag modification
4. **Update scan policies** - Use tags to target specific asset groups
5. **Build dashboards** - Filter widgets by new tags
6. **Set remediation SLAs** - Define patching windows based on tags
7. **Train team members** - Explain tag meanings and usage
8. **Document tag strategy** - Save agent report for future reference
9. **Schedule quarterly review** - Re-run agent every 90 days

## Getting Help

- **Issues:** [GitHub Issues](https://github.com/dpickenstenable/tenable-tag-optimizer-agent/issues)
- **Documentation:** See README.md for comprehensive details
- **Tenable Support:** [Tenable Community](https://community.tenable.com/)
- **Claude Code:** [claude.ai/code](https://claude.ai/code)

---

**Ready to start?** Open Claude Code and say:

```
Run the Tenable Tag Optimizer
```

The agent will handle the rest!
