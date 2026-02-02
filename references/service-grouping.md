# Service Grouping Patterns

## Overview

Campaigns in Google Ads accounts are often organized by service/product line, with multiple campaign types (Search, Display, Performance Max, Video) supporting the same business objective. Analyzing campaigns grouped by service provides better insights than analyzing each campaign in isolation.

## Campaign Naming Convention Patterns

### Common Patterns to Detect

**Pattern 1: [Brand/Prefix] - [Service] - [Channel]**
```
Examples:
- "EWS - Mortgage - Search"
- "EWS - Mortgage - Display"
- "ABC Plumbing - Water Heaters - Search"
- "ABC Plumbing - Water Heaters - Display"

Grouping: Extract middle segment as service name
```

**Pattern 2: [Service] - [Channel]**
```
Examples:
- "Auto Loans - Search"
- "Auto Loans - Display"
- "HVAC Repair - Search"

Grouping: Extract first segment as service name
```

**Pattern 3: [Brand] - [Service] [Channel]**
```
Examples:
- "EWS - Mortgage Search"
- "EWS - Mortgage Display"
- "EWS - Free Checking - Search"

Grouping: Match service keywords, ignore channel suffix
```

**Pattern 4: [Service]_[Channel]_[Modifier]**
```
Examples:
- "Mortgage_Search_Brand"
- "Mortgage_Display_Remarketing"
- "AutoLoans_PMax_2024"

Grouping: Extract first segment as service
```

### Channel Identifiers to Strip

When parsing campaign names, remove these suffixes/keywords to identify the service:

```
Search, Display, PMax, Performance Max, Video, YouTube,
Remarketing, RLSA, DSA, Brand, Non-Brand, Competitor,
Responsive, Dynamic, Shopping, Local, App
```

### Date/Version Identifiers to Ignore

```
Q1, Q2, Q3, Q4, 2024, 2025, 2026, Jan, Feb, etc.
v1, v2, test, new, old, updated, revised
```

## Industry-Specific Grouping Guidelines

### Financial Services (Banks, Credit Unions)

**Typical Services:**
| Service | Common Campaign Keywords |
|---------|-------------------------|
| Mortgage | mortgage, home loan, home buying |
| Home Equity | HELOC, home equity, home eq, HEL |
| Auto Loans | auto loan, car loan, vehicle financing |
| Auto Refinance | auto refi, car refinance, vehicle refi |
| Personal Loans | personal loan, signature loan |
| Checking | checking, free checking, checking account |
| Savings | savings, money market, CD, certificate |
| Credit Cards | credit card, rewards card |
| Membership | become a member, join, membership |

**Grouping Questions to Ask:**
- Should HELOC and Home Equity be grouped? (Usually yes)
- Should Auto Loans and Auto Refinance be grouped? (Often no - different customer journey)
- Should Business and Personal checking be grouped? (Usually no - different audiences)

### Home Services (Plumbers, HVAC, Electricians)

**Typical Services:**
| Service | Common Campaign Keywords |
|---------|-------------------------|
| Emergency | emergency, urgent, 24/7, same day |
| Repair | repair, fix, broken, not working |
| Installation | install, new, replacement |
| Maintenance | maintenance, tune-up, service, inspection |
| [Specific Service] | water heater, AC, furnace, drain, electrical |

**Grouping Questions to Ask:**
- Should Emergency and Repair be grouped? (Sometimes - depends on business model)
- Should service-specific campaigns be grouped by service or by job type?

### Legal Services

**Typical Services:**
| Service | Common Campaign Keywords |
|---------|-------------------------|
| Personal Injury | PI, injury, accident, car accident |
| Family Law | divorce, custody, family |
| Criminal Defense | criminal, DUI, defense |
| Estate Planning | estate, will, trust, probate |
| Business Law | business, corporate, contract |

**Grouping Questions to Ask:**
- Should sub-specialties be grouped? (e.g., all personal injury types together)
- Should brand and non-brand campaigns be grouped with service campaigns?

### Healthcare

**Typical Services:**
| Service | Common Campaign Keywords |
|---------|-------------------------|
| [Specialty] | cardiology, orthopedics, dermatology |
| Primary Care | primary care, family medicine, general |
| Urgent Care | urgent care, walk-in, immediate |
| [Procedure] | knee replacement, cataract, colonoscopy |

### E-commerce/Retail

**Typical Services:**
| Service | Common Campaign Keywords |
|---------|-------------------------|
| [Product Category] | shoes, electronics, furniture |
| [Brand] | brand-specific campaigns |
| Sales/Promotions | sale, clearance, black friday |
| Generic | generic category terms |

## Clarification Questions by Scenario

### Similar Product Names

When campaigns have similar but not identical names:

```
"Should these be grouped together?"
- HELOC Search + Home Equity Display
- Auto Loans Search + Auto Refinance PMax
- Business Checking + Commercial Checking
- Free Checking + Premium Checking
```

### Brand vs Non-Brand

```
"How should brand campaigns be handled?"
Options:
1. Group brand campaigns with their service (Mortgage Brand + Mortgage Search)
2. Keep all brand campaigns as separate "Brand" service
3. Exclude brand from service analysis entirely
```

### Remarketing Campaigns

```
"How should remarketing campaigns be handled?"
Options:
1. Group with parent service (Mortgage RLSA + Mortgage Search)
2. Keep as separate "Remarketing" service
3. Analyze separately due to different customer stage
```

### Competitor Campaigns

```
"How should competitor targeting campaigns be handled?"
Options:
1. Group with the service they're competing for
2. Keep as separate "Competitor" service
3. Exclude from service analysis
```

### Generic/Broad Campaigns

```
"This campaign targets broad terms. Which service does it belong to?"
- Campaign: "General Services - Search"
- Options: [List all detected services] + "Keep as standalone"
```

## Service Health Assessment

### Healthy Multi-Channel Service

Characteristics:
- Multiple channels active and spending
- All channels generating conversions
- Combined CPA meets target
- Channel CPAs within 2x of each other
- Positive or stable MoM trends

### Warning Signs

| Pattern | Indication | Action |
|---------|------------|--------|
| One channel 0 conv, other converting | Channel mismatch | Pause or restructure underperformer |
| Display CPA > 3x Search CPA | Inefficient support | Evaluate Display value |
| PMax taking all conversions | Channel cannibalization | May not need Search/Display |
| Search declining, Display stable | Market shift | Investigate Search issues |
| All channels declining | Service-level problem | Review offer, landing page, market |

### Critical Service Failures

| Pattern | Severity | Action |
|---------|----------|--------|
| All channels $0 spend | Critical | Check campaign settings |
| All channels 0 conversions | Critical | Check conversion tracking |
| Significant spend, 0 conversions across all | Critical | Pause, investigate |

## Output: Presenting Service Groupings

After detection, present groupings to user like this:

```
I detected the following service groupings based on campaign names:

**Mortgage** (2 campaigns)
- Mortgage Search
- Mortgage Display

**Home Equity** (2 campaigns)
- HELOC Search
- Home Equity Display

**Auto Loans** (1 campaign)
- Auto Loans Search

**Auto Refinance** (1 campaign)
- Auto Refi PMax

**Checking** (3 campaigns)
- Free Checking Search
- Business Checking Search
- Business Checking Display

**Standalone** (1 campaign)
- Become A Member Display

Questions:
1. Should HELOC and Home Equity be combined into one "Home Equity" service?
2. Should the Checking campaigns be kept separate or grouped together?
3. Does "Become A Member" belong with any service or stay standalone?
```

## Remembering Client Preferences

After confirming groupings, note the pattern for future analyses:

```
Client: [Client Name]
Service Grouping Rules:
- HELOC + Home Equity = "Home Equity" service
- Auto Loans ≠ Auto Refinance (keep separate)
- Business Checking ≠ Personal Checking (keep separate)
- Brand campaigns: Group with service
- Remarketing: Group with service
```
