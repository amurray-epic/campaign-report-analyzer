---
name: campaign-report-analyzer
description: |
  Analyze Google Ads campaign reports to identify performance issues, budget opportunities, and generate executive summaries with tiered optimization recommendations.
  Use this skill when: (1) Analyzing campaign performance CSV exports from Google Ads, (2) Identifying underperforming campaigns or services,
  (3) Generating tiered optimization recommendations, (4) Grouping campaigns by service/product line for holistic analysis,
  (5) Detecting red flags like zero conversions with spend or significant MoM declines.
  Triggers on: Google Ads campaign data, campaign performance analysis, budget optimization, MoM analysis, service-level reporting.
---

# Campaign Report Analyzer

Analyze Google Ads campaign reports to identify performance issues, budget opportunities, and generate executive summaries with tiered optimization recommendations. Supports grouping campaigns by service/product line for holistic analysis.

## Quick Start

1. Collect client context (business name, goals, custom benchmarks)
2. Read the uploaded CSV campaign report
3. Parse and normalize the data (handle UTF-16 encoding if present)
4. Identify and confirm service/product groupings with user
5. Apply analysis methodology at both service and campaign level
6. Generate executive summary with tiered recommendations

## Required User Input

Before analysis, collect:

### Basic Information
- **Business name**: Client company name for report header
- **Industry**: For benchmark selection (e.g., financial services, home services, legal, retail)
- **Business goals**: Primary objectives (leads, sales, awareness, etc.)
- **Custom CTR benchmarks** (optional): Client-specific thresholds by campaign type
  - If not provided, use industry defaults from [references/benchmarks.md](references/benchmarks.md)

### Service Groupings
After reviewing campaign names, ask the user to confirm how campaigns should be grouped by service/product line.

**Why group by service?**
- Multiple campaign types (Search, Display, PMax) often support the same product/service
- Service-level analysis reveals true performance of a product line
- Identifies channel synergies and gaps (e.g., Search converting but Display not)
- Enables better budget allocation decisions across the full customer journey

**How to identify potential groupings:**
1. Parse campaign names for common keywords (e.g., "Mortgage", "Auto", "HVAC")
2. Look for patterns like "[Brand] - [Service] - [Channel]"
3. Present detected groupings to user for confirmation
4. Ask clarifying questions for ambiguous cases (e.g., "Should HELOC and Home Equity be grouped?")

## CSV Format Expected

Google Ads campaign exports typically include these columns (handle variations in naming):

| Column | Variations | Description |
|--------|------------|-------------|
| Campaign | Campaign name | Name of the campaign |
| Campaign status | Status | Enabled, Paused, etc. |
| Campaign type | Type | Search, Display, Performance Max, Video |
| Budget | Daily budget | Daily budget amount |
| Cost | Spend | Total spend in period |
| Impr. | Impressions | Number of impressions |
| Clicks | - | Number of clicks |
| CTR | Click-through rate | Clicks / Impressions |
| Conversions | Conv. | Number of conversions |
| Cost / conv. | CPA | Cost per conversion |
| Conv. rate | Conversion rate | Conversions / Clicks |
| Search impr. share | Impr. share | % of available impressions received |

**Note**: Google Ads exports may be UTF-16 encoded with spaces between characters. Handle this encoding during parsing.

## Analysis Methodology

### Step 1: Data Preparation

```
1. Detect and handle file encoding (UTF-16, UTF-8, etc.)
2. Parse CSV, normalize column names
3. Convert metrics to numbers (remove currency symbols, percentages, quotes)
4. Handle "Compare to" columns for MoM analysis
5. Filter to active campaigns (exclude "Total:" summary rows)
6. Categorize campaigns by type (Search, Display, PMax, Video)
```

### Step 2: Collect Client Context

Ask user for:
- Business name
- Industry (for benchmark selection)
- Primary business goals
- Custom CTR benchmarks (or use defaults)

### Step 3: Identify Service Groupings

**Auto-detect potential services from campaign names:**

```
1. Extract service keywords from campaign names
   - Look for patterns: [Prefix] - [Service] - [Channel]
   - Common delimiters: " - ", "_", "|"

2. Group campaigns by detected service keyword

3. Present groupings to user for confirmation:
   "I detected these service groupings. Please confirm or adjust:"

   - Mortgage: [Campaign A - Search], [Campaign B - Display]
   - Auto Loans: [Campaign C - Search]
   - Home Equity: [Campaign D - Search], [Campaign E - Display]

4. Ask clarifying questions for ambiguous cases:
   - "Should [X] and [Y] be grouped together?"
   - "Is [Campaign Z] a standalone service or part of [Service]?"
```

**Common grouping scenarios to ask about:**
- Products with similar names (HELOC vs Home Equity, Auto Loans vs Auto Refi)
- B2B vs B2C versions of same service (Business Checking vs Personal Checking)
- Brand campaigns vs product campaigns
- Remarketing campaigns (group with parent service or separate?)

### Step 4: Service-Level Analysis

For each service group, calculate combined metrics:

```
Service Spend = Sum of all campaign costs in group
Service Conversions = Sum of all campaign conversions in group
Service CPA = Service Spend / Service Conversions
Service Impressions = Sum of impressions (note: may have overlap in Display/PMax)
```

**Evaluate service health:**

| Status | Criteria |
|--------|----------|
| Healthy | Combined CPA meets goals, positive MoM trend, all channels contributing |
| Warning | CPA rising OR one channel underperforming OR declining MoM |
| Critical | Zero conversions with spend OR major channel failure |
| Mixed | Some channels healthy, others critical (needs rebalancing) |

**Identify channel synergy issues:**
- Search converting but Display not → Display may need pause or restructure
- Display efficient but Search struggling → May indicate brand awareness working
- PMax dominating conversions → Evaluate if Search/Display still needed

### Step 5: Campaign Health Assessment

Evaluate each individual campaign against benchmarks and flag issues.

See [references/benchmarks.md](references/benchmarks.md) for default thresholds.

**Performance Scoring:**

| Score | Criteria |
|-------|----------|
| Healthy | Meets or exceeds benchmarks, positive/stable MoM trends |
| Warning | Below benchmarks OR declining MoM (>20% drop) |
| Critical | Significantly below benchmarks AND zero conversions with spend |

### Step 6: Identify Red Flags

Apply red flag detection rules from [references/red-flags.md](references/red-flags.md).

**Priority Red Flags:**

| Issue | Detection Criteria | Severity |
|-------|-------------------|----------|
| Zero conversions with spend | Cost > $50 AND Conversions = 0 | Critical |
| Significant performance drop | Any key metric down >20% MoM | High |
| Low impression share | Search impr. share < 20% | Medium |
| Underspending | Actual spend < 50% of budget capacity | Medium |
| Poor CTR | CTR below campaign-type benchmark | Medium |
| Channel imbalance | One channel in service has 0 conv, another converting | High |

### Step 7: Budget Analysis

Focus on **underspending** detection:

```
Daily Budget Capacity = Budget × Days in Period
Utilization Rate = Cost / Daily Budget Capacity

Flag if:
- Utilization Rate < 50% (significantly underspending)
- Utilization Rate < 80% AND campaign is performing well (opportunity)
```

**Service-level budget analysis:**
- Calculate total budget capacity for service
- Identify if budget is optimally distributed across channels
- Flag services where high-performing channels are budget-limited while low-performers have unused budget

**Underspending Causes to Note:**
- Low search volume / limited audience
- Bid strategy constraints
- Ad scheduling limitations
- Targeting too narrow
- Low Quality Score (for Search)

### Step 8: Month-over-Month Analysis

Compare current period to previous period for significant changes (>20%):

| Metric | Positive Change | Negative Change |
|--------|-----------------|-----------------|
| Cost | May need monitoring | Budget constraints? |
| Impressions | Growth opportunity | Reach declining |
| Clicks | Engagement improving | Traffic dropping |
| CTR | Better relevance | Ad fatigue or competition |
| Conversions | Strategy working | Investigate immediately |
| Conv. rate | Efficiency improving | Landing page or audience issue |
| CPA | Efficiency improving | Costs rising |

**Change Calculation:**
```
Change % = ((Current - Previous) / Previous) × 100
Flag if: |Change %| > 20%
```

Perform MoM analysis at both service level and campaign level.

### Step 9: Generate Tiered Recommendations

Categorize recommendations by urgency:

**Critical (Act Now):**
- Services/campaigns with zero conversions and significant spend (>$100)
- Major performance drops (>50% decline in key metrics)
- Campaigns with conversion tracking issues
- Complete service failures (all channels non-functional)

**High Priority (This Week):**
- Performance declining >20% MoM
- Underspending on high-performing campaigns/services
- Low impression share on converting campaigns
- Channel imbalance within a service (reallocate budget)

**Optimization Opportunities (This Month):**
- CTR below benchmark but converting
- Budget reallocation between services or channels
- Ad copy/creative refresh suggestions
- Audience or targeting adjustments
- Adding channel support (e.g., Display for Search-only service)

## Output Format

Generate an executive summary structured as:

```markdown
# Campaign Performance Report

**Client**: [Business Name]
**Industry**: [Industry]
**Report Period**: [Date Range]
**Analysis Date**: [Today's Date]

---

## Executive Summary

[2-3 sentence overview of account health and key findings]

### Account Snapshot

| Metric | Current | Previous | Change |
|--------|---------|----------|--------|
| Total Spend | $X | $X | +/-X% |
| Total Conversions | X | X | +/-X% |
| Avg. CPA | $X | $X | +/-X% |
| Avg. CTR | X% | X% | +/-X% |

### Service Health Overview

| Service | Channels | Spend | Conv. | CPA | MoM | Status |
|---------|----------|-------|-------|-----|-----|--------|
| [Service 1] | Search + Display | $X | X | $X | +/-X% | Healthy |
| [Service 2] | Search | $X | X | $X | +/-X% | Warning |
| [Service 3] | PMax | $X | X | $X | +/-X% | Critical |

---

## Critical Issues (Act Now)

### [Issue Name]
**Service/Campaign**: [Name]
**Impact**: [$ amount or metric affected]
**Recommendation**: [Specific action to take]

---

## High Priority (This Week)

### [Issue Name]
**Service/Campaign**: [Name]
**Finding**: [What was found]
**Recommendation**: [Action to take]

---

## Optimization Opportunities (This Month)

- [Opportunity 1]
- [Opportunity 2]
- [Opportunity 3]

---

## Service-by-Service Breakdown

### [Service Name]
**Channels**: [List of campaign types]
**Combined Performance**:

| Metric | Current | Previous | Change |
|--------|---------|----------|--------|
| Spend | $X | $X | +/-X% |
| Conversions | X | X | +/-X% |
| CPA | $X | $X | +/-X% |

**Channel Breakdown**:

| Channel | Spend | Conv. | CPA | CTR | Notes |
|---------|-------|-------|-----|-----|-------|
| Search | $X | X | $X | X% | [Note] |
| Display | $X | X | $X | X% | [Note] |

**Assessment**: [Healthy/Warning/Critical/Mixed - explanation]

**Recommendations**: [Service-specific recommendations]

[Repeat for each service]

---

## Standalone Campaigns

[Campaigns that don't fit into service groups]

| Campaign | Type | Spend | Conv. | CPA | CTR | Status | Notes |
|----------|------|-------|-------|-----|-----|--------|-------|
| [Name] | [Type] | $X | X | $X | X% | [Status] | [Note] |

---

## Next Steps Checklist

### Critical (Do Today)
- [ ] [Critical action 1]
- [ ] [Critical action 2]

### High Priority (This Week)
- [ ] [High priority action 1]
- [ ] [High priority action 2]

### Optimization (This Month)
- [ ] [Optimization action 1]
- [ ] [Optimization action 2]

### Follow-Up
- [ ] Schedule next review: [Date - 2 weeks out]
```

## Campaign Type Considerations

### Search Campaigns
- Focus on CTR, conversion rate, impression share
- Quality Score impacts (if visible in data)
- Keyword-level issues may need Search Terms report
- Often the "workhorse" channel for direct response

### Display Campaigns
- Lower CTR expectations (benchmark: 0.5-1%)
- Focus on view-through conversions if available
- Supports Search by building awareness
- Should be evaluated in context of service, not alone

### Performance Max Campaigns
- Limited visibility into performance drivers
- Focus on overall conversion efficiency
- May cannibalize Search/Display - monitor for this
- Asset group performance (if available)

### Video Campaigns
- View rate and engagement metrics
- Cost per view analysis
- Brand awareness vs. direct response goals
- Often supports other channels in the service

## Channel Synergy Patterns

**Healthy multi-channel service:**
- Search captures high-intent traffic, converts efficiently
- Display builds awareness, provides incremental conversions at reasonable CPA
- Combined CPA is acceptable, both channels contribute

**Warning signs:**
- One channel has zero conversions while other converts → evaluate or pause
- Display CPA is 3x+ Search CPA → may not be worth the spend
- Search declining while Display stable → audience may be shifting

**Recommendations by pattern:**
- Search-only service performing well → consider adding Display support
- Display not converting → pause and reallocate to Search
- PMax outperforming Search/Display → evaluate if legacy channels still needed

## References

- [references/benchmarks.md](references/benchmarks.md) - Default CTR and performance benchmarks by campaign type and industry
- [references/red-flags.md](references/red-flags.md) - Detailed red flag detection rules and severity levels
- [references/service-grouping.md](references/service-grouping.md) - Patterns for identifying and grouping campaigns by service
