# Tenable Tag Optimizer Setup Guide

Complete installation and configuration instructions for the Tenable Tag Optimizer Agent.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Authentication Setup](#authentication-setup)
- [Verification](#verification)
- [Automation Setup](#automation-setup)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required

✅ **Claude Code** - AI-powered development environment  
   - Install from [claude.ai/code](https://claude.ai/code)
   - Available as CLI, desktop app, web app, or IDE extension

✅ **Tenable Account** with one of:
   - Tenable Vulnerability Management (TVM)
   - Tenable One / Tenable Exposure Management

✅ **API Permissions** - Your Tenable user account needs:
   - **Tags: "Can Manage"** permission (required to create tags)
   - **Assets: "Can View"** permission (required to analyze inventory)

✅ **Git** installed on your system

### Optional (Recommended)

⭐ **Tenable MCP Server** - Simplifies authentication and enhances agent capabilities  
   - Install: [github.com/tenable/mcp-server-tenable](https://github.com/tenable/mcp-server-tenable)

---

## Installation Steps

### Step 1: Clone the Repository

```bash
# Clone from GitHub
git clone https://github.com/dpickenstenable/tenable-tag-optimizer-agent.git
cd tenable-tag-optimizer-agent
```

### Step 2: Install the Agent

Copy the agent file to your Claude Code agents directory:

**macOS / Linux:**
```bash
cp agent.md ~/.claude/agents/tenable-tag-optimizer.md

# Verify installation
ls -la ~/.claude/agents/tenable-tag-optimizer.md
```

**Windows (PowerShell):**
```powershell
Copy-Item agent.md $env:USERPROFILE\.claude\agents\tenable-tag-optimizer.md

# Verify installation
Get-Item $env:USERPROFILE\.claude\agents\tenable-tag-optimizer.md
```

### Step 3: Verify Claude Code Installation

```bash
# Check Claude Code version
claude --version

# Should return something like: Claude Code CLI v2.1.0
```

If Claude Code is not installed, visit [claude.ai/code](https://claude.ai/code) for installation instructions.

---

## Authentication Setup

You have **two authentication options**. Choose the one that fits your workflow:

### Option 1: Tenable MCP Server (Recommended)

The Tenable MCP Server handles authentication automatically and provides enhanced API capabilities.

#### Install Tenable MCP Server

```bash
# Using npm
npm install -g @tenable/mcp-server-tenable

# Or using npx (no global install)
npx @tenable/mcp-server-tenable
```

#### Configure MCP Server

1. Generate Tenable API keys:
   - Log into Tenable VM or Tenable One
   - Navigate to **Settings → My Account → API Keys**
   - Click **"Generate"** and save your Access Key and Secret Key

2. Configure Claude Code to use the MCP server:

   Edit your Claude Code config file:
   - **macOS/Linux:** `~/.claude/config.json`
   - **Windows:** `%USERPROFILE%\.claude\config.json`

   Add the Tenable MCP server configuration:

   ```json
   {
     "mcpServers": {
       "tenable": {
         "command": "npx",
         "args": [
           "-y",
           "@tenable/mcp-server-tenable"
         ],
         "env": {
           "TENABLE_ACCESS_KEY": "YOUR_ACCESS_KEY_HERE",
           "TENABLE_SECRET_KEY": "YOUR_SECRET_KEY_HERE",
           "TENABLE_URL": "https://cloud.tenable.com"
         }
       }
     }
   }
   ```

   **For EU cloud customers:** Replace `TENABLE_URL` with `https://cloud.eu.tenable.com`

3. Restart Claude Code:
   ```bash
   # CLI users
   claude restart

   # Desktop app users: quit and relaunch the app
   ```

4. Verify MCP connection:
   ```bash
   # In Claude Code, type:
   > List available Tenable tools

   # You should see Tenable MCP tools listed
   ```

---

### Option 2: Direct API Authentication

If you prefer not to use the MCP server, you can authenticate directly with environment variables.

#### Set Environment Variables

**macOS / Linux (bash/zsh):**

Add to your `~/.bashrc`, `~/.zshrc`, or `~/.bash_profile`:

```bash
export TENABLE_ACCESS_KEY="your_access_key_here"
export TENABLE_SECRET_KEY="your_secret_key_here"
export TENABLE_URL="https://cloud.tenable.com"
```

Then reload your shell:
```bash
source ~/.bashrc  # or ~/.zshrc
```

**Windows (PowerShell):**

Set persistent environment variables:

```powershell
[Environment]::SetEnvironmentVariable("TENABLE_ACCESS_KEY", "your_access_key_here", "User")
[Environment]::SetEnvironmentVariable("TENABLE_SECRET_KEY", "your_secret_key_here", "User")
[Environment]::SetEnvironmentVariable("TENABLE_URL", "https://cloud.tenable.com", "User")
```

Restart PowerShell after setting variables.

**Windows (Command Prompt):**

```cmd
setx TENABLE_ACCESS_KEY "your_access_key_here"
setx TENABLE_SECRET_KEY "your_secret_key_here"
setx TENABLE_URL "https://cloud.tenable.com"
```

Restart Command Prompt after setting variables.

---

## Verification

### Test the Agent Installation

Open Claude Code and run:

```
> Run the Tenable Tag Optimizer
```

**Expected output:**
```
✓ Connecting to Tenable...
✓ Retrieving asset inventory...
✓ Analyzing existing tags...
✓ Detecting patterns...

[Agent proceeds with analysis]
```

### Test Tenable API Access

Verify your credentials work:

**With MCP Server:**
```
> List my Tenable tags

# Should return your existing tag categories
```

**With Direct API:**
```bash
# macOS/Linux
curl -X GET \
  -H "X-ApiKeys: accessKey=$TENABLE_ACCESS_KEY; secretKey=$TENABLE_SECRET_KEY" \
  https://cloud.tenable.com/tags/categories

# Windows (PowerShell)
$headers = @{
  "X-ApiKeys" = "accessKey=$env:TENABLE_ACCESS_KEY; secretKey=$env:TENABLE_SECRET_KEY"
}
Invoke-RestMethod -Uri "https://cloud.tenable.com/tags/categories" -Headers $headers
```

**Expected response:** JSON array of your existing tag categories

### Verify Permissions

Check that your account has the required permissions:

1. Log into Tenable
2. Navigate to **Settings → Access Control → Groups**
3. Find your user group
4. Under **Tags**, verify **"Can Manage"** is enabled
5. Under **Assets**, verify **"Can View"** is enabled

If these aren't enabled, contact your Tenable administrator.

---

## Automation Setup

Run the Tag Optimizer on a schedule to keep your tag taxonomy up-to-date as your infrastructure evolves.

### Quarterly Cron Job (Linux/macOS)

```bash
# Edit crontab
crontab -e

# Add this line to run on the 1st of Jan, Apr, Jul, Oct at 2 AM
0 2 1 1,4,7,10 * cd /path/to/tenable-tag-optimizer-agent && claude run "Run the Tenable Tag Optimizer" >> /var/log/tag-optimizer.log 2>&1
```

### Launchd (macOS - Preferred)

Create `~/Library/LaunchAgents/com.tenable.tag-optimizer.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.tenable.tag-optimizer</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/claude</string>
        <string>run</string>
        <string>Run the Tenable Tag Optimizer</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Month</key>
        <integer>1</integer>
        <key>Day</key>
        <integer>1</integer>
        <key>Hour</key>
        <integer>2</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>StandardOutPath</key>
    <string>/tmp/tag-optimizer.log</string>
    <key>StandardErrorPath</key>
    <string>/tmp/tag-optimizer-error.log</string>
    <key>EnvironmentVariables</key>
    <dict>
        <key>TENABLE_ACCESS_KEY</key>
        <string>YOUR_ACCESS_KEY</string>
        <key>TENABLE_SECRET_KEY</key>
        <string>YOUR_SECRET_KEY</string>
    </dict>
</dict>
</plist>
```

Load the job:
```bash
launchctl load ~/Library/LaunchAgents/com.tenable.tag-optimizer.plist
```

### Task Scheduler (Windows)

1. Open **Task Scheduler**
2. Click **Create Task**
3. **General Tab:**
   - Name: "Tenable Tag Optimizer"
   - Description: "Quarterly tag taxonomy review"
   - Run whether user is logged on or not
4. **Triggers Tab:**
   - New trigger → Monthly
   - Months: January, April, July, October
   - Days: 1
   - Start time: 2:00 AM
5. **Actions Tab:**
   - Action: Start a program
   - Program: `C:\Program Files\Claude\claude.exe`
   - Arguments: `run "Run the Tenable Tag Optimizer"`
   - Start in: `C:\path\to\tenable-tag-optimizer-agent`
6. **Conditions Tab:**
   - Uncheck "Start the task only if the computer is on AC power"
7. Click **OK** and enter your Windows password

### GitHub Actions (CI/CD)

Create `.github/workflows/tag-optimization.yml` in your repo:

```yaml
name: Quarterly Tag Optimization

on:
  schedule:
    # Run on the 1st of Jan, Apr, Jul, Oct at 02:00 UTC
    - cron: '0 2 1 1,4,7,10 *'
  workflow_dispatch:  # Allow manual triggering

jobs:
  optimize-tags:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Claude Code
        run: |
          curl -fsSL https://install.claude.ai/cli.sh | sh
          
      - name: Run Tag Optimizer
        env:
          TENABLE_ACCESS_KEY: ${{ secrets.TENABLE_ACCESS_KEY }}
          TENABLE_SECRET_KEY: ${{ secrets.TENABLE_SECRET_KEY }}
          TENABLE_URL: ${{ secrets.TENABLE_URL }}
        run: |
          claude run "Run the Tenable Tag Optimizer"
```

Add secrets to your GitHub repo:
1. Settings → Secrets and variables → Actions
2. Add: `TENABLE_ACCESS_KEY`, `TENABLE_SECRET_KEY`, `TENABLE_URL`

---

## Troubleshooting

### Issue: "Command 'claude' not found"

**Cause:** Claude Code is not installed or not in PATH

**Solution:**
```bash
# Verify installation
which claude

# If not found, reinstall Claude Code
curl -fsSL https://install.claude.ai/cli.sh | sh

# Reload shell
source ~/.bashrc  # or ~/.zshrc
```

### Issue: "403 Forbidden - Insufficient permissions"

**Cause:** Your Tenable account doesn't have "Can Manage" permission on Tags

**Solution:**
1. Contact your Tenable administrator
2. Request "Can Manage" permission under Tags in Access Control
3. Wait 5-10 minutes for permissions to propagate
4. Retry the agent

### Issue: "Agent file not found"

**Cause:** Agent wasn't copied to the correct directory

**Solution:**
```bash
# Check if the file exists
ls -la ~/.claude/agents/tenable-tag-optimizer.md

# If not found, copy it again
cp agent.md ~/.claude/agents/tenable-tag-optimizer.md
```

### Issue: "Unable to connect to Tenable API"

**Cause:** Invalid credentials or network issue

**Solution:**

Test API connectivity:
```bash
# Test with curl
curl -v -X GET \
  -H "X-ApiKeys: accessKey=$TENABLE_ACCESS_KEY; secretKey=$TENABLE_SECRET_KEY" \
  https://cloud.tenable.com/session

# Should return 200 OK with your user info
```

If this fails:
- Verify your API keys are correct
- Check if you're using the right URL (`cloud.tenable.com` vs `cloud.eu.tenable.com`)
- Ensure your network allows HTTPS to Tenable
- Try regenerating your API keys in Tenable UI

### Issue: "MCP server not responding"

**Cause:** MCP server not configured correctly

**Solution:**

1. Check MCP server configuration:
   ```bash
   cat ~/.claude/config.json
   ```

2. Verify Tenable MCP server is installed:
   ```bash
   npm list -g @tenable/mcp-server-tenable
   ```

3. Test MCP server directly:
   ```bash
   npx @tenable/mcp-server-tenable
   ```

4. Check MCP server logs:
   ```bash
   tail -f ~/.claude/logs/mcp-tenable.log
   ```

5. Restart Claude Code after config changes

### Issue: "No patterns detected" warning

**Cause:** Your environment has:
- Very few assets (<100)
- Inconsistent naming conventions
- Already comprehensive tag coverage

**Solution:** This isn't necessarily an error!

- **Small environments:** Consider manually creating tags
- **Inconsistent naming:** Review and standardize hostname patterns
- **Complete coverage:** You may already have optimal tags

### Issue: Rate limit errors (429)

**Cause:** Too many API requests in a short time

**Solution:** The agent automatically implements exponential backoff. Just wait for it to retry.

If persistent:
- Run the agent outside business hours
- Contact Tenable support to increase your API rate limit

---

## Getting Help

**GitHub Issues:** [github.com/dpickenstenable/tenable-tag-optimizer-agent/issues](https://github.com/dpickenstenable/tenable-tag-optimizer-agent/issues)

**Tenable Community:** [community.tenable.com](https://community.tenable.com/)

**Claude Code Support:** [claude.ai/code](https://claude.ai/code)

---

## Next Steps

After successful setup:

✅ Run your first tag optimization  
✅ Review the recommendations  
✅ Create approved tags  
✅ Schedule quarterly automation  
✅ Configure tag governance policies  
✅ Train your team on the new taxonomy

---

**Happy Tagging! 🏷️**
