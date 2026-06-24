# Installation & Usage Guide

Complete guide for installing and running the Tenable Exposure Impact Simulator in Claude Code.

## What You Need

1. **Claude Code** - CLI, desktop app, or web interface at claude.ai/code
2. **Tenable access** - MCP server configured or API credentials
3. **Asset Exposure Scores (AES)** - Available in Tenable One/Exposure Management

## What This Agent Does

The Exposure Impact Simulator answers "what if" questions **before** you remediate vulnerabilities:

- "If I patch this CVE, how much will my AES drop?"
- "What's the fastest way to get all assets below AES 900?"
- "I have 20 hours this week - what gives maximum risk reduction?"
- "Should I focus on crown jewels or CISA KEVs?"

**Result:** Make data-driven remediation decisions based on predicted impact, not guesses.

## Installation

### Option 1: Install from Repository (Recommended)

If this agent is available in the claude-gsd repository:

```bash
# From Claude Code prompt
/install tenable-exposure-impact-simulator
```

The agent will be automatically installed to `~/.claude/agents/`

### Option 2: Manual Installation

1. Clone or download this repository:
   ```bash
   cd ~/.claude/agents
   git clone https://github.com/dpickenstenable/tenable-exposure-impact-simulator.git
   ```

2. Copy the agent definition file:
   ```bash
   cp tenable-exposure-impact-simulator/tenable-exposure-impact-simulator.md ~/.claude/agents/
   ```

3. Verify installation:
   ```bash
   ls ~/.claude/agents/tenable-exposure-impact-simulator.md
   ```

## Pre-Installation Setup

### Configure Tenable Access (Required)

This agent requires Tenable One (Exposure Management) with Asset Exposure Scores enabled.

#### Option A: Using Tenable MCP Server (Easiest)

If you have Tenable MCP server configured in `~/.claude/config.json`, the agent will automatically use it.

Verify MCP connection:
```bash
# In Claude Code
Check if my Tenable MCP server is connected
```

#### Option B: Using Tenable API Credentials

Generate API keys:
1. Login to Tenable.io → Settings → My Account → API Keys
2. Click "Generate API Key"
3. Name: "Exposure Impact Simulator"
4. Permissions: Read Assets, Read Vulnerabilities
5. Copy Access Key and Secret Key

Set environment variables:
```bash
# Add to ~/.bashrc or ~/.zshrc
export TENABLE_ACCESS_KEY="your-access-key"
export TENABLE_SECRET_KEY="your-secret-key"
export TENABLE_URL="https://cloud.tenable.com"
```

Reload shell:
```bash
source ~/.bashrc  # or ~/.zshrc
```

### Verify AES Data Availability

The agent requires Asset Exposure Scores from Tenable One:

```bash
# In Claude Code
Check if my Tenable environment has AES scores enabled
```

If AES is not enabled, contact Tenable to enable Exposure Management features.

## How to Run the Agent

The agent runs **inside Claude Code** conversations using natural language.

### Step 1: Start Claude Code

Open Claude Code in your preferred interface:
- **CLI**: Run `claude` in your terminal
- **Desktop**: Launch the Claude Code app
- **Web**: Visit claude.ai/code

### Step 2: Run the Agent

In the Claude Code conversation, use natural language:

```
Run the Tenable Exposure Impact Simulator
```

**Or be more specific:**

```
Use the exposure-impact-simulator to predict AES reduction 
if I patch CVE-2024-1234 on prod-db-01
```

**Behind the scenes**, Claude Code executes:
```javascript
Agent({
  subagent_type: "Tenable Exposure Impact Simulator",
  prompt: "Simulate AES impact of patching CVE-2024-1234 on prod-db-01"
})
```

> **Note**: You don't type the JavaScript code yourself - just tell Claude Code in natural language what you want to simulate, and it will invoke the agent for you.

### What Happens When You Run It

1. **Connects to Tenable** - Via MCP server or API credentials
2. **Retrieves baseline data** - Current AES scores, vulnerabilities, CES
3. **Runs simulation** - Calculates predicted impact of remediation
4. **Generates report** - Shows before/after, confidence level, recommendations
5. **Compares scenarios** - If multiple options requested

## Complete Example Walkthrough

### Scenario: Deciding what to patch this week

**Step 1**: Start Claude Code
```bash
claude
```

**Step 2**: Ask the simulation question
```
I have 20 hours this week to patch vulnerabilities. 

Run the Tenable Exposure Impact Simulator to show me:
1. What's the maximum CES reduction I can achieve?
2. Which vulnerabilities should I prioritize?
3. What will our CES be after these fixes?
```

**Step 3**: Agent execution (automatic)
```
Agent: Connected to Tenable One environment
✓ Current CES: 487
✓ Assets: 437 total, 47 crown jewels (AES > 900)
✓ Vulnerabilities: 1,247 total (342 critical, 573 high)

Running optimization algorithm...

Budget-Optimized Plan (20 hours):

Week Plan:
1. prod-db-01: CVE-2024-1234 (2h) → AES 948→712 (-236)
2. prod-app-03: CVE-2024-5678 (3h) → AES 914→723 (-191)
3. prod-web-07: CVE-2023-9999 (2h) → AES 892→701 (-191)
... (8 more vulnerabilities)

Summary:
- Total effort: 19.5 hours (within budget!)
- CES reduction: 487 → 441 (-46 points, -9.4%)
- Crown jewels removed: 3 assets drop below AES 900
- Vulnerabilities resolved: 12 critical, 3 high

Cumulative Impact:
Week 0: CES 487
Week 1: CES 441 (this week's work)
Week 2: CES 412 (if continue at same pace)
Week 3: CES 389 (target CES < 400 achieved!)

Recommendation: Focus on this plan - maximum ROI for your time budget.
```

**Step 4**: Ask follow-up questions
```
What if I focus on CISA KEVs instead? Compare that strategy.
```

```
Show me detailed impact analysis for prod-db-01 specifically
```

```
What's the confidence level of these predictions?
```

## Advanced Usage Examples

### Single Asset Impact Prediction

```
Simulate the AES impact if I patch CVE-2024-1234 on prod-db-01

Show me:
- Current AES and vulnerability breakdown
- Predicted new AES after patching
- Confidence level
- Estimated effort
- Whether it removes crown jewel status
```

### Strategy Comparison

```
Compare three remediation strategies:

Strategy A: Fix all crown jewels first (AES > 900)
Strategy B: Fix all CISA KEV vulnerabilities first
Strategy C: Fix vulnerabilities by VPR score (highest first)

Show which gives:
- Fastest CES reduction
- Most efficient use of time
- Best crown jewel impact
```

### Budget-Constrained Optimization

```
I have 40 hours this month for vulnerability remediation.

Use the Exposure Impact Simulator with optimization algorithm to find:
- Maximum possible CES reduction
- Ordered list of vulnerabilities to fix
- Expected timeline and milestones
- Comparison vs. random selection
```

### Crown Jewel Targeting

```
Find the minimum remediation effort needed to:
- Get all assets below AES 900 (remove crown jewel status)
- Prioritize by ease of remediation
- Show expected timeline

Current crown jewels: 47 assets
```

### Historical Trend Analysis

```
Show me our CES trajectory over the last 6 months

Include:
- Actual CES changes week-by-week
- Major remediation events
- Remediation velocity (CES points/week)
- Projected trend if we continue at current pace
```

### Cumulative Impact Simulation

```
I'm planning to patch these vulnerabilities in order:
1. CVE-2024-1111 on prod-db-01
2. CVE-2024-2222 on prod-app-03
3. CVE-2024-3333 on prod-web-07

Simulate cumulative impact:
- AES after each fix
- CES after each fix
- Show diminishing returns if any
```

### ROI and Budget Justification

```
Calculate ROI for hiring one additional security engineer:

Current state:
- Remediation velocity: 15 CES points/month
- Engineer capacity: 160 hours/month
- Current CES: 487

Show:
- Projected velocity with additional engineer
- Time to reach CES < 400 (current vs. with hire)
- Business risk reduction value
```

### Incident Response Planning

```
We just discovered a new critical vulnerability affecting 50 assets.

Simulate emergency patching scenarios:
1. Patch all 50 immediately (how much effort? impact?)
2. Patch top 10 by AES first (quick wins?)
3. Patch by asset criticality tags

Which approach gives best risk reduction soonest?
```

### Maintenance Window Planning

```
We have a 4-hour maintenance window this Saturday for prod-db cluster (3 servers).

What vulnerabilities can we realistically fix in 4 hours?
Show:
- Recommended vulnerability list
- Expected AES/CES impact
- Rollback time considerations
```

## Understanding the Output

### Impact Summary

The agent shows predicted impact for each remediation:

```
┌─────────────────────────────────────────────────┐
│ Remediation Impact: prod-db-01                  │
├─────────────────────────────────────────────────┤
│ Vulnerability: CVE-2024-1234 (Apache Struts)    │
│ Current AES:           948                      │
│ Predicted New AES:     712 (±5%)                │
│ Reduction:             -236 points (-25%)       │
│                                                 │
│ Crown Jewel Status: REMOVED ⭐                  │
│ Confidence: High                                │
│ Estimated Effort: 2 hours                       │
│                                                 │
│ CES Impact: 487 → 482 (-5 points)              │
└─────────────────────────────────────────────────┘
```

### Optimization Results

For budget-constrained scenarios:

```
20-Hour Budget Optimization:

Priority | Asset          | CVE          | Effort | AES Impact | CES/Hour
---------|----------------|--------------|--------|------------|----------
1        | prod-db-01     | CVE-2024-1234| 2h     | -236       | 2.5
2        | prod-app-03    | CVE-2024-5678| 3h     | -191       | 2.1
3        | prod-web-07    | CVE-2023-9999| 2h     | -191       | 1.9
...

Total: 12 vulnerabilities, 19.5 hours, CES 487 → 441 (-46 points)
```

**Key metric:** CES/Hour shows efficiency - higher is better ROI.

### Confidence Levels

- **High (±5%)**: Single critical vuln, well-understood impact, historical data
- **Medium (±10%)**: Multiple vulns, moderate complexity, some unknowns
- **Low (±20%)**: Complex scenarios, many dependencies, no historical data

### Strategy Comparison

When comparing approaches:

```
Strategy Comparison:

Strategy A: Crown Jewels First
- CES Reduction: 487 → 452 (-35 points)
- Effort: 28 hours
- Crown Jewels Resolved: 12 assets
- Efficiency: 1.25 CES points/hour

Strategy B: CISA KEV First
- CES Reduction: 487 → 461 (-26 points)  
- Effort: 18 hours
- Crown Jewels Resolved: 3 assets
- Efficiency: 1.44 CES points/hour ✓ BEST

Strategy C: Top VPR Scores
- CES Reduction: 487 → 465 (-22 points)
- Effort: 15 hours
- Crown Jewels Resolved: 2 assets
- Efficiency: 1.47 CES points/hour ✓ MOST EFFICIENT

Recommendation: Strategy B balances impact and effort for quick wins.
```

### Trajectory Visualization

ASCII chart showing predicted CES over time:

```
CES Trajectory (12-week projection):

500 ┤●                                          
487 ┤ ╲                                         
    │  ╲                                        
470 │   ●                                       
    │    ╲                                      
450 │     ●                                     
    │      ╲                                    
430 │       ●                                   
    │        ╲___                               
410 │            ●                              
    │             ╲                             
390 │              ●  ← Target CES < 400       
    └──────────────────────────────             
    Week0  Week3  Week6  Week9  Week12         

Projected target achievement: Week 11
```

## Troubleshooting

### "Agent not found" or "Unknown subagent_type"

**Problem**: Agent isn't installed correctly

**Solution**:
```bash
# Verify installation
ls -la ~/.claude/agents/tenable-exposure-impact-simulator.md

# If missing, reinstall
cd ~/.claude/agents
git clone https://github.com/dpickenstenable/tenable-exposure-impact-simulator.git
cp tenable-exposure-impact-simulator/tenable-exposure-impact-simulator.md .
```

### "Tenable API Authentication Failed"

**Problem**: Invalid Tenable credentials

**Solution**:
1. Verify API keys are correct
2. Check keys haven't expired
3. Ensure keys have Read Assets and Read Vulnerabilities permissions
4. Test MCP connection if using MCP server

### "AES scores not available"

**Problem**: Your Tenable instance doesn't have Exposure Management enabled

**Solution**:
- This agent requires Tenable One (Exposure Management)
- Contact your Tenable account team to enable AES scoring
- Alternatively, use traditional vulnerability prioritization agents

### "No vulnerabilities found for asset"

**Problem**: Asset exists but has no scan results

**Solution**:
1. Verify asset has been scanned recently
2. Check scan completed successfully
3. Ensure scan includes vulnerability detection (not just discovery)

### Predictions seem inaccurate

**Problem**: Model hasn't been calibrated to your environment

**Solution**:
1. Check confidence level - low confidence = higher variance
2. After remediations, compare predicted vs actual AES
3. Provide feedback: "Prediction was off by X%"
4. Agent learns from feedback to improve future predictions

### Optimization taking too long

**Problem**: Too many assets/vulnerabilities to analyze

**Solution**:
1. Narrow scope: "Optimize for production assets only"
2. Filter by severity: "Only critical and high vulnerabilities"
3. Limit asset count: "Top 50 assets by AES"

## Real-World Usage Patterns

### Weekly Sprint Planning

**Every Monday:**
```
Good morning! Run the Exposure Impact Simulator for sprint planning.

Context:
- Sprint duration: 2 weeks
- Team capacity: 80 hours
- Focus: Production environment
- Goal: Maximum CES reduction

Generate optimized sprint backlog.
```

### Monthly Executive Report

**End of month:**
```
Generate monthly exposure management report:

1. CES trend this month (start vs. end)
2. Number of vulnerabilities remediated
3. Crown jewel status changes
4. Projected CES for next 3 months if we continue current pace
5. Recommended acceleration if behind target
```

### Quarterly Planning

**Quarterly OKR setting:**
```
We want to achieve CES < 400 by end of Q2.

Current: CES 487, Q1 ending
Time remaining: 13 weeks
Team: 3 engineers, 120 hours/week total

Questions:
1. Is this goal achievable?
2. What's the required weekly velocity?
3. What strategy should we follow?
4. Do we need additional resources?
```

### Emergency Zero-Day Response

**When zero-day announced:**
```
New zero-day: CVE-2024-XXXX affecting Apache Struts

Questions:
1. How many of our assets are affected?
2. What's the AES impact if we don't patch?
3. What's the effort to patch all affected assets?
4. Can we prioritize by AES and patch highest-risk first?
5. Show 24-hour emergency plan vs. 7-day normal plan
```

### Audit Preparation

**Before compliance audit:**
```
Audit in 30 days. Need to demonstrate vulnerability management maturity.

Generate:
1. 6-month CES trend showing improvement
2. Evidence of risk-based prioritization
3. Remediation velocity metrics
4. Projected state at audit date
5. Remediation plans for any remaining high-risk items
```

## Integration Examples

### With Patch Management Tools

```
I'm planning to deploy patches via WSUS this weekend.

Simulate impact of these patches:
- MS-2024-01: Affects 45 Windows servers
- MS-2024-02: Affects 23 Windows servers  
- MS-2024-03: Affects 67 Windows servers

Should I deploy all, or prioritize subset for maximum AES impact?
```

### With ServiceNow/Jira

```
Create prioritized vulnerability remediation tickets for ServiceNow:

1. Export top 20 vulnerabilities by predicted CES impact
2. Include estimated effort for each
3. Include predicted AES/CES changes
4. Format as CSV for import into ServiceNow

Fields: Asset, CVE, Current AES, Predicted AES, Effort, Priority
```

### With Slack Alerts

```
Set up weekly Slack alert with:
- Current CES
- CES change from last week  
- Top 5 quick wins (high impact, low effort)
- Progress toward quarterly CES target

Send to #security-ops channel every Monday 9 AM
```

## Security & Privacy

### Credential Handling

- ✅ Credentials read from environment variables or MCP config
- ✅ Used only during execution, never logged
- ✅ All API calls use HTTPS/TLS encryption
- ✅ Read-only access - agent never modifies data

### Best Practices

- Use read-only API keys
- Minimum required permissions (Read Assets, Read Vulnerabilities)
- Rotate credentials every 90 days
- Use MCP server for centralized credential management

### Data Privacy

- All processing happens locally in Claude Code
- No vulnerability data sent to external services
- Predictions calculated locally
- Reports saved to local filesystem only

## Tips for Best Results

✅ **DO**:
- Provide context about your constraints (time, resources, priorities)
- Ask for multiple scenarios to compare options
- Check confidence levels - high confidence = more reliable
- Use optimization algorithms for budget-constrained planning
- Compare predicted vs. actual after remediation to improve accuracy

❌ **DON'T**:
- Trust low-confidence predictions for critical decisions
- Forget to account for testing/validation time in effort estimates
- Optimize purely by CES - consider business criticality too
- Ignore crown jewel status - AES > 900 assets need priority
- Skip validation - always check predictions against actual results

## Getting Help

- **Full Documentation**: See `README.md`
- **Algorithm Details**: See README for AES estimation formula
- **Tenable API Docs**: https://developer.tenable.com/
- **Tenable One Docs**: https://docs.tenable.com/vulnerability-management/Content/Settings/exposure-score.htm

## Common Questions

**Q: How accurate are the predictions?**
A: High-confidence predictions are typically ±5%. Accuracy improves as the agent learns from your remediation patterns.

**Q: Can I simulate patching across multiple assets?**
A: Yes! Ask for bulk simulations: "Simulate patching CVE-2024-1234 across all affected assets"

**Q: Does this work with Tenable.sc (SecurityCenter)?**
A: Currently requires Tenable One (cloud) with AES scoring. Tenable.sc support planned for future version.

**Q: Can I export results to Excel?**
A: Yes! Ask for CSV export: "Export optimization results as CSV"

**Q: How does this account for asset criticality?**
A: CES weights high-AES assets more heavily (3x for crown jewels vs. 0.5x for low-AES).

---

**Ready to start?**

1. Set up Tenable access (required)
2. Verify AES data is available
3. Open Claude Code and say: `"Run the Tenable Exposure Impact Simulator"`

Start predicting and optimizing your remediation impact!
