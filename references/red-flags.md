# Red Flag Detection Rules

## Critical Red Flags (Immediate Action Required)

### 1. Zero Conversions with Significant Spend

**Detection:**
```
Cost > $100 AND Conversions = 0
```

**Severity:** Critical

**Possible Causes:**
- Conversion tracking broken
- Wrong audience targeting
- Poor landing page experience
- Irrelevant keywords/placements
- Misleading ad copy

**Recommended Actions:**
1. Verify conversion tracking is firing
2. Check landing page for issues
3. Review search terms (for Search campaigns)
4. Consider pausing until investigated

### 2. Conversion Tracking Issues

**Detection:**
```
Campaign has historical conversions but current period = 0
OR
All campaigns show 0 conversions (account-wide issue)
```

**Severity:** Critical

**Recommended Actions:**
1. Check Google Tag Manager / conversion tags
2. Verify conversion actions in Google Ads
3. Test conversion tracking with tag assistant
4. Review recent website changes

### 3. Massive Performance Drop

**Detection:**
```
Any key metric declined >50% MoM:
- Conversions down >50%
- CTR down >50%
- Impression share down >50%
```

**Severity:** Critical

**Possible Causes:**
- Algorithm/bid strategy issues
- Budget constraints
- Competitor activity
- Seasonality
- Account/campaign changes

**Recommended Actions:**
1. Check change history in Google Ads
2. Review competitor landscape
3. Analyze by device, location, time
4. Consider manual CPC temporarily

---

## High Priority Red Flags (Address This Week)

### 4. Declining Performance Trends

**Detection:**
```
Key metrics down 20-50% MoM:
- Conversions declining
- CTR declining
- Conversion rate declining
- Impression share declining
```

**Severity:** High

**Investigation Steps:**
1. Identify when decline started
2. Correlate with any changes made
3. Check for external factors (seasonality, competition)
4. Segment data to find problem areas

### 5. Rising Cost Per Conversion

**Detection:**
```
CPA increased >30% MoM
OR
CPA exceeds target by >50%
```

**Severity:** High

**Possible Causes:**
- Increased competition
- Audience fatigue
- Bid strategy issues
- Quality Score decline
- Conversion rate dropping

**Recommended Actions:**
1. Review bid strategy settings
2. Check Quality Scores (Search)
3. Refresh ad creative
4. Tighten targeting

### 6. Low Impression Share on Converting Campaigns

**Detection:**
```
Conversions > 5 AND Search Impr. Share < 30%
```

**Severity:** High

**Rationale:** Campaign is converting but missing potential traffic.

**Recommended Actions:**
1. Increase budget if limited by budget
2. Raise bids if limited by rank
3. Improve Quality Score
4. Review targeting restrictions

### 7. High-Performing Campaign Underspending

**Detection:**
```
Conversion Rate > account average
AND Budget Utilization < 60%
```

**Severity:** High

**Recommended Actions:**
1. Expand keyword targeting
2. Broaden audience targeting
3. Increase bids
4. Add more ad variations

---

## Medium Priority Red Flags (Address This Month)

### 8. General Underspending

**Detection:**
```
Actual Spend < 50% of (Budget × Days in Period)
```

**Severity:** Medium

**Possible Causes:**
- Low search volume
- Bids too low
- Targeting too narrow
- Low Quality Score
- Ad scheduling restrictions

**Investigation Steps:**
1. Check impression share breakdown
2. Review bid strategy and targets
3. Analyze targeting settings
4. Check ad scheduling

### 9. Below-Benchmark CTR

**Detection:**
```
Search CTR < 3%
Display CTR < 0.5%
PMax CTR < 1%
Video CTR < 0.5%
```

**Severity:** Medium

**Possible Causes:**
- Poor ad relevance
- Weak ad copy
- Wrong audience
- Ad fatigue
- Strong competition

**Recommended Actions:**
1. Review and refresh ad copy
2. Test new headlines/descriptions
3. Check keyword relevance
4. Review audience targeting

### 10. Budget-Limited Campaigns

**Detection:**
```
Status contains "Limited by budget"
OR
Budget Utilization > 95% consistently
```

**Severity:** Medium (or High if converting well)

**Recommended Actions:**
1. Evaluate campaign performance
2. Increase budget if ROI positive
3. Reduce bids to stretch budget
4. Improve targeting efficiency

### 11. High Click Volume, Low Conversions

**Detection:**
```
Clicks > 100 AND Conversions < 2 AND Conversion Rate < 1%
```

**Severity:** Medium

**Possible Causes:**
- Landing page issues
- Traffic quality issues
- Misaligned expectations
- Wrong audience

**Investigation Steps:**
1. Review landing page experience
2. Check for Search campaign: search terms
3. Check for Display: placement report
4. Analyze by device, location

---

## Low Priority Red Flags (Monitor)

### 12. Minor Performance Fluctuations

**Detection:**
```
Metrics changed 10-20% MoM
```

**Severity:** Low

**Action:** Note in report, continue monitoring

### 13. New Campaign with Limited Data

**Detection:**
```
Campaign < 30 days old AND Conversions < 10
```

**Severity:** Low

**Action:** Note limited data, recommend patience, schedule follow-up

### 14. Display/Video Campaign Low CTR

**Detection:**
```
Display/Video CTR below benchmark but conversions present
```

**Severity:** Low

**Action:** Note as optimization opportunity, not urgent

---

## Red Flag Priority Matrix

Use this matrix to prioritize findings in the report:

| Severity | Response Time | Report Section |
|----------|---------------|----------------|
| Critical | Immediate | "Critical Issues (Act Now)" |
| High | Within 1 week | "High Priority (This Week)" |
| Medium | Within 1 month | "Optimization Opportunities" |
| Low | Next review cycle | "Monitoring Notes" |

## Combining Red Flags

When multiple red flags affect the same campaign:

1. **Escalate severity** if 2+ issues present
2. **Prioritize by impact** (conversions > spend > engagement)
3. **Group related issues** in recommendations
4. **Identify root cause** that may explain multiple symptoms

Example:
```
Campaign X has:
- Zero conversions (Critical)
- Declining CTR (High)
- Low impression share (Medium)

Combined assessment: Critical - likely targeting or tracking issue
Recommendation: Pause campaign, investigate conversion tracking first
```

## Account-Level Red Flags

Check for patterns across all campaigns:

| Pattern | Indication |
|---------|------------|
| All campaigns 0 conversions | Tracking issue |
| All campaigns declining | Account-level problem or market shift |
| All campaigns underspending | Bid strategy or targeting too restrictive |
| Cost up, conversions flat | Efficiency declining, competition rising |

## Seasonal Considerations

Before flagging MoM changes, consider:

| Industry | Peak Seasons | Off Seasons |
|----------|--------------|-------------|
| Retail | Nov-Dec, holidays | Jan-Feb |
| Tax services | Jan-Apr | May-Dec |
| HVAC | Summer, Winter extremes | Spring, Fall |
| Legal | Generally stable | Holiday weeks |
| Finance | Q1, year-end | Summer |

Note in report if analysis period may be affected by seasonality.
