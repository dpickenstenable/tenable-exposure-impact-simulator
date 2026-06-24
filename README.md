# Tenable Exposure Impact Simulator

**What-if analysis engine for AES/CES remediation impact prediction**

The Exposure Impact Simulator helps security teams answer "what if" questions before remediating vulnerabilities. It predicts Asset Exposure Score (AES) and Cyber Exposure Score (CES) changes, enabling data-driven prioritization and resource optimization.

## 🚀 New User? Start Here

**[→ Installation & Usage Guide (INSTALL.md)](INSTALL.md)** - Complete walkthrough for installing and running this agent in Claude Code

Already installed? Continue with Quick Start below.

---

## 🎯 What It Does

- **Predict AES/CES Impact** - Know before you patch how much risk reduction you'll achieve
- **Optimize Remediation Sequences** - Find the fastest path to your target CES
- **Compare Strategies** - Crown jewels vs. CISA KEVs vs. top VPR scores
- **Maximize ROI** - Get the best risk reduction per hour of effort
- **Budget Planning** - "I have 20 hours this week, what should I fix?"

---

## 🚀 Quick Start

### Installation

See [INSTALL.md](INSTALL.md) for detailed installation instructions.

### Usage

Open Claude Code and use natural language:

```
Run the Tenable Exposure Impact Simulator
```

**Or be more specific:**
```
Use the exposure-impact-simulator to predict AES reduction 
if I patch CVE-2024-1234 on prod-db-01
```

> **Note**: You interact with the agent using natural language in Claude Code. Claude Code automatically handles the agent invocation - you don't need to type any JavaScript code yourself.

**Example conversation:**
```
Agent: Connected to your Tenable environment. What would you like to simulate?

You: "Show me the top 5 highest-impact single fixes"
```

---

## 💡 Example Queries

### Single Asset Impact
```
"If I patch CVE-2024-1234 on prod-db-01, what's the AES impact?"
```

### Scenario Comparison
```
"Compare three strategies: fix crown jewels, fix CISA KEVs, fix top 10 VPR scores"
```

### Optimization
```
"I have 20 hours this week. What's the maximum CES reduction I can achieve?"
```

### Crown Jewel Targeting
```
"What's the fastest way to get all assets below AES 900?"
```

### Cumulative Simulation
```
"Show cumulative impact if I fix vulnerabilities in this order: CVE-1, CVE-2, CVE-3"
```

---

## 🧮 How It Works

### AES Estimation Algorithm

The agent uses a **weighted heuristic model** that factors in:

- **VPR Score** - Vulnerability Priority Rating (base weight)
- **Severity** - Critical (2.0x), High (1.5x), Medium (1.0x), Low (0.5x)
- **Exploit Availability** - Known exploit = 1.5x multiplier
- **Vulnerability Age** - Older vulns have higher impact (up to 1.5x)
- **Patch Availability** - No patch = 1.4x multiplier

**Formula:**
```
vulnerability_weight = VPR × severity × exploit × age × patch

AES_reduction = current_AES × (removed_weight / total_weight) ^ 0.85
```

The 0.85 exponent accounts for diminishing returns.

### CES Calculation

**Cyber Exposure Score** is the weighted average of all asset AES scores:

```
CES = Σ(asset_AES × asset_weight) / Σ(asset_weight)

asset_weight:
  - AES > 900 (Crown Jewel): 3.0
  - AES 700-900 (High): 2.0
  - AES 400-700 (Medium): 1.0
  - AES < 400 (Low): 0.5
```

### Confidence Levels

- **High (±5%)**: Single critical vuln, historical data available
- **Medium (±10%)**: Multiple vulns, moderate complexity
- **Low (±20%)**: Complex scenarios, no historical data

---

## 🎨 Output Formats

### Text Summary (Default)
```
┌─────────────────────────────────────────────────┐
│ Remediation Impact: prod-db-01                  │
├─────────────────────────────────────────────────┤
│ Current AES:           948                      │
│ Estimated New AES:     712                      │
│ Reduction:             -236 points (-25%)       │
│                                                 │
│ Confidence: High (±5%)                          │
│ Estimated Effort: 2 hours                       │
│                                                 │
│ ⚡ REMOVES CROWN JEWEL STATUS                   │
└─────────────────────────────────────────────────┘
```

### JSON Export
```json
{
  "baseline": {"ces": 487, "crown_jewels": 47},
  "scenario": {"name": "Fix prod-db-01", "method": "single_asset"},
  "final_state": {
    "ces": 471,
    "ces_reduction": -16,
    "confidence": "high"
  }
}
```

### ASCII Graph
```
CES Trajectory:

500 ┤                                          
487 ┤●                                         
    │ ╲                                        
470 │  ●                                       
    │   ╲                                      
450 │    ●                                     
    └─────────────────────────────             
    Week0  Week1  Week2                        
```

---

## 🔧 Advanced Features

### 1. Optimization Algorithms

**Greedy CES-per-Hour**: Maximizes risk reduction for fixed time budget
- Calculates efficiency (CES reduction / effort hours) for each vulnerability
- Selects highest-efficiency items first
- O(n log n) complexity

**Knapsack Dynamic Programming**: Optimal subset selection
- Finds mathematically optimal remediation set
- Respects time/resource constraints
- O(n × W) complexity

**Crown Jewel Minimum Set**: Removes crown jewel status with minimum effort
- Finds smallest vulnerability set to drop AES below 900
- Orders assets by remediation difficulty

### 2. Historical Trend Analysis
```
"Show me our CES trajectory over the last 6 months"
```
- Plots actual CES changes
- Overlays remediation events
- Calculates remediation velocity

### 3. Predictive Modeling
```
"If we maintain current pace, when will we hit CES < 400?"
```
- Projects future trajectory
- Identifies required acceleration
- Shows milestone dates

### 4. Budget Justification
```
"Show ROI for hiring one more security engineer"
```
- Current vs. projected remediation velocity
- CES improvement over 1 year
- Business risk reduction

---

## 📊 Use Cases

### Security Operations
- **Daily Triage**: "What should I prioritize today?"
- **Sprint Planning**: "What can we achieve in this 2-week sprint?"
- **Resource Allocation**: "Do we need more engineers?"

### Management Reporting
- **Executive Briefings**: "Here's our quarterly CES trajectory"
- **Budget Requests**: "We need $X to achieve Y risk reduction"
- **Audit Preparation**: "Show progress toward compliance target"

### Incident Response
- **Emergency Patching**: "Which vulns give us the biggest quick wins?"
- **Containment Planning**: "What's the impact of isolating these assets?"

---

## 🔌 Integration

### Tenable API

The agent connects to Tenable via:

**Method 1: MCP Server (Recommended)**
- Requires Tenable MCP Server running
- Automatic authentication

**Method 2: Direct API**
```bash
export TENABLE_ACCESS_KEY="your-access-key"
export TENABLE_SECRET_KEY="your-secret-key"
```

### Required API Endpoints

- `tenable_one_search_assets` - Asset inventory with AES scores
- `tenable_one_search_findings` - Vulnerability data with VPR
- `plugins_get_plugin_details` - Detailed plugin information

---

## 🧪 Accuracy & Validation

### Model Validation

The weighted heuristic model is calibrated from:
- Tenable research on AES calculation factors
- Historical remediation patterns
- Industry best practices

### Future ML Enhancement

The agent collects remediation event data:
```json
{
  "before_state": {"aes": 948, "vuln_count": 47},
  "remediation": {"vulns_fixed": ["plugin-123"], "effort": 2},
  "prediction": {"estimated_aes": 712},
  "actual_aes": 718  # collected from next scan
}
```

After 100+ events, train ML model for higher accuracy.

---

## 📈 Example Results

**Real-world scenario:**
- Company: 500 assets, CES 512
- Remediation budget: 40 hours/week
- Target: CES < 400 by Q2

**Simulator output:**
```
Optimized 12-Week Plan:
Week 1-4: Crown Jewel Focus (CES 512 → 467)
Week 5-8: High-VPR Sweep (CES 467 → 428)
Week 9-12: Infrastructure Hardening (CES 428 → 392)

Target achieved: Week 11 (1 week ahead of schedule)
Total effort: 420 hours (10.5 hours/week average)
Assets improved: 147
Vulnerabilities resolved: 892 critical/high
```

---

## 🛠️ Customization

### Custom Effort Estimates
```
"Simulate fixing CVE-2024-1234 on prod-db-01 assuming 4 hours effort"
```

### Custom Weights
```
"Prioritize by business impact instead of AES"
```
(Requires asset tags: `BusinessImpact:high`)

### Custom Targets
```
"Find minimum effort to get production assets average AES below 600"
```

---

## 🔒 Security & Privacy

- **Read-Only**: Agent never modifies Tenable data
- **No Credentials Stored**: Uses environment variables or MCP Server
- **Local Processing**: All calculations run locally
- **No External Calls**: Simulation runs entirely on your data

---

## 📞 Support & Documentation

- **Full Agent Code**: `tenable-exposure-impact-simulator.md`
- **Algorithm Details**: See agent documentation
- **Tenable API Docs**: https://developer.tenable.com/

---

## 📜 License

MIT License - See LICENSE file

---

## 🤝 Contributing

This agent is part of the Tenable CyberAgents Exchange. Contributions welcome!

1. Fork the repository
2. Make your changes
3. Submit a pull request
4. Add examples to the documentation

---

## 🎯 Roadmap

**Version 1.0** (Current)
- ✅ Weighted heuristic AES estimation
- ✅ CES calculation
- ✅ Greedy optimization
- ✅ Scenario comparison
- ✅ Budget-constrained planning

**Version 1.1** (Planned)
- 🔄 ML model training from historical data
- 🔄 Network topology integration (attack path simulation)
- 🔄 Business impact modeling
- 🔄 ServiceNow/Jira export

**Version 2.0** (Future)
- 🔮 Real-time monitoring dashboard
- 🔮 Automated remediation workflows
- 🔮 Multi-tenant support
- 🔮 API for external integrations

---

**Predict, optimize, and maximize your remediation impact with confidence!** 🎯
