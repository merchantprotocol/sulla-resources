# Lead Prospecting Skill

## Purpose
Discover and qualify local businesses matching the Ideal Customer Profile (ICP) for outreach. Focus on geographic area: Coeur d'Alene Idaho, North Idaho, and Spokane Washington.

## ICP Criteria (10-point scoring system)

### Must-Have Requirements (0 if fail)
- B2B (business-to-business)
- 2-200 employees
- Has a website
- Has decision-maker structure (not solo practitioner)

### Scoring Factors
| Factor | Points | Description |
|--------|--------|-------------|
| Industry Match | +2 | Target industries: SaaS, Professional Services, Healthcare Tech, Manufacturing, Construction, Real Estate |
| Company Size Fit | +2 | 10-100 employees = 2pts, 2-10 or 100-200 = 1pt |
| Has Website | +2 | Functional business website required |
| US-Based | +1 | Primary operations in United States |
| Growth Signals | +1 | Hiring (job listings), recent funding, expansion mentions |
| Modern Tech | +1 | Uses modern tech stack, has developer presence on GitHub, has API |

**Minimum Score: 5/10 to proceed**

## Geographic Focus

### Primary Markets
- **Coeur d'Alene, ID** - Primary target city
- **North Idaho** - Bonner County, Kootenai County, Shoshone County, Boundary County
- **Spokane, WA** - Adjacent metro area, 30 minutes east

### Secondary Markets
- Lewiston, ID
- Missoula, MT (偶们)
- Tri-Cities, WA

## Data Sources

### Search Engines (rate limited)
- **Google Maps**: 1 req/5sec, 20 results per search
- **Google Search**: 1 req/3sec, for company research

### Business Directories
- **Yelp**: 1 req/3sec, max 100/day
- **Merchant Circle**: 1 req/5sec
- **Yellowpages**: 1 req/5sec
- **BBB (Better Business Bureau)**: 1 req/5sec
- **Nextdoor Business**: 1 req/5sec

### Funding/Startup Databases
- **Crunchbase**: 1 req/10sec (60min cooldown after 10)
- **AngelList**: 1 req/5sec

### Social/Developer
- **GitHub**: 60 req/hr, use authenticated if available
- **LinkedIn**: Manual browser only, no scraping

### Local/Regional
- **Idaho Business Directory**: local idaho business listings
- **Spokane Journal of Business**: regional business news/directory
- **Local Chamber of Commerce sites**: Coeur d'Alene, Spokane, Sandpoint, etc.

## Lead Discovery Keywords

### Industry Searches
```
coeur d'alene saas companies
spokane professional services companies
north idaho manufacturing businesses
kootenai county tech companies
spokane healthcare tech startups
idaho construction companies
coeur d'alene real estate companies
```

### Directory-Specific Searches
```
site:yelp.com "Coeur d'Alene" "Company"
site:merchantcircle.com "Coeur d'Alene"
site:bbb.org "Coeur d'Alene" "A rating"
site:linkedin.com "Coeur d'Alene" "CEO"
```

### Growth Signal Searches
```
"Coeur d'Alene" "hiring" "job posting"
"north idaho" " Series funding"
"Spokane" "acquired" "2024"
"Idaho" "expanding" "new location"
```

## Rate Limiting Rules

### Critical: Never Hit Rate Limits
- Pay attention to rate limits so we don't get identified as a bot
- We have all the time in the world - go slow, be patient

### Per-Source Limits
| Source | Delay | Daily Max |
|--------|-------|-----------|
| Google Maps | 5 sec | Unlimited (be reasonable) |
| Yelp | 3 sec | 100 |
| Crunchbase | 10 sec | 20 |
| GitHub | N/A | 60/hr |
| Hunter.io (email) | 3 sec | 50/day |
| LinkedIn | Manual only | N/A |

### Batch Processing
- Max 20 businesses processed through full research per day
- Space out discovery across days to avoid patterns

## Twenty CRM Integration

### Company Fields
- `name`: Business name
- `domainName`: Website domain
- `address`: Full address
- `employees`: Estimated employee count
- `linkedinLink`: LinkedIn company URL
- `xLink`: Twitter/X company URL
- `icpScore`: 0-10 score
- `icpReasoning`: Why they passed/failed
- `researchStatus`: discovered → researching → researched → verified
- `dataSources`: Comma-separated list of where data came from
- `redFlags`: Any concerns (no website, wrong industry, etc.)
- `geoArea`: coeur_dalene, north_idaho, spokane, other

### Person Fields
- `firstName`, `lastName`: Full name
- `email`: Verified email
- `phone`: Phone number
- `jobTitle`: Current title
- `linkedinLink`: Personal LinkedIn
- `xLink`: Personal Twitter/X
- `company`: Link to company record
- `isDecisionMaker`: true/false
- `decisionLevel`: C-level, VP, Director, Manager
- `contactConfidence`: 0-10 (10 = complete data)
- `researchStatus`: discovered → researching → researched → verified
- `verificationNotes`: Research notes

## Workflow Integration

### Lead Prospecting Workflow (workflow-lead-prospecting)
1. Load ICP from this skill
2. Parallel search 6 data sources
3. Merge and deduplicate results
4. Score against ICP (min 5/10)
5. Create company records in Twenty CRM
6. Queue for business research

### Business Research Workflow (workflow-business-research)
1. Query CRM for companies where researchStatus='discovered'
2. Research website, Google, social profiles in parallel
3. Identify decision makers (owner, CEO, manager)
4. Create person records for decision makers
5. Update company researchStatus='researched'

### Individual Research Workflow (workflow-individual-research)
1. Query CRM for people where researchStatus='discovered'
2. Validate identity (LinkedIn, Google search)
3. Research LinkedIn, X, contact finder, Google in parallel
4. Update person with contact info
5. Update researchStatus='researched'

## Output Files

Daily logs written to:
- `~/sulla/daily-logs/{date}/lead-prospecting/companies-created.md`
- `~/sulla/daily-logs/{date}/lead-prospecting/research-queue.md`
- `~/sulla/daily-logs/{date}/lead-prospecting/completion.md`
- `~/sulla/daily-logs/{date}/lead-prospecting/individual-research.md`
