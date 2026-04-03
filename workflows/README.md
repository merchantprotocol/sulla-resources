# Sulla Workflows Library

Production-ready automation workflows for lead generation, prospecting, and quality assurance.

## Active Workflows

### 1. Google Maps Lead Generation (`google-maps-lead-gen`)

**Purpose:** Automated lead extraction from Google Maps with intelligent filtering and enrichment.

**Trigger:** Manual or scheduled (Mon/Wed/Fri 10 AM PT)

**Workflow Steps:**
1. **Start Lead Gen** - Extract business category and location from user request
2. **Search Google Maps** - Collect business listings with contact data, ratings, reviews
3. **Enrich Lead Data** - Visit websites, extract social profiles, calculate lead scores (0-100)
4. **Save Lead Profiles** - Create structured markdown files with full business analysis
5. **Log & Report** - Generate summary with top opportunities and next steps

**Key Features:**
- Filters by review count, website presence, business status
- Calculates lead scores based on rating, reviews, social presence, website quality
- Identifies service opportunities: web design, SEO, social media, content marketing, PPC, email marketing
- Saves to `~/sulla/projects/leads/{category}/`
- Updates master tracker automatically

**Usage:**
```
"Generate leads for marketing agencies in Los Angeles"
"Find dentists with no website in Miami"
"Search for restaurants with low reviews in San Francisco"
```

---

### 2. Social Media Prospecting (`social-media-prospects`)

**Purpose:** Find businesses with weak social media presence and create prioritized outreach lists.

**Trigger:** Manual or scheduled (Tue/Thu 2 PM PT)

**Workflow Steps:**
1. **Start Social Prospecting** - Extract target industry and location
2. **Discover Target Businesses** - Find 30-50 prospects from multiple sources (Google Maps, Yelp, directories)
3. **Audit Social Media Presence** - Comprehensive audit across 7 platforms (Facebook, Instagram, LinkedIn, Twitter, TikTok, YouTube, Pinterest)
4. **Assess Service Opportunity** - Calculate opportunity scores, recommend services, estimate deal value
5. **Create Prospect Profiles** - Detailed markdown profiles with audit results and pitch angles
6. **Generate Outreach List** - CSV exports, email templates, DM scripts, call scripts, CRM import files
7. **Report & Log** - Pipeline summary with top prospects and common weaknesses

**Key Features:**
- Scores each platform 0-10 based on activity, engagement, completeness
- Calculates overall social_score (0-100) and opportunity_score (0-100)
- Tiers prospects: Tier 1 (immediate), Tier 2 (this week), Tier 3 (nurture), Tier 4 (monitor)
- Estimates deal values: $2k-5k/mo (small), $5k-15k/mo (medium), $15k-50k/mo (enterprise)
- Generates complete outreach toolkit

**Usage:**
```
"Find social media prospects in the restaurant industry"
"Prospect retail businesses with weak Instagram presence"
"Generate leads for social media management services"
```

---

### 3. Verify Fixes (`verify-fixes`)

**Purpose:** Automated testing and validation of bug fixes with comprehensive reporting.

**Trigger:** Manual, scheduled (daily 6 AM PT), or event-driven (deployment, PR merge, issue closed)

**Workflow Steps:**
1. **Start Verification** - Extract test suites and issue IDs to verify
2. **Load Test Definitions** - Read and validate test suite configurations
3. **Setup Test Environment** - Prepare isolated environment, run smoke tests
4. **Execute Test Suites** - Run all tests with retry logic, capture detailed results
5. **Analyze Test Results** - Identify patterns, root causes, regressions, flaky tests
6. **Generate Test Report** - Multi-format reports (markdown, JUnit XML, JSON, GitHub comments)
7. **Update Issue Tracker** - Close verified fixes, reopen failures, create new issues
8. **Cleanup Test Environment** - Archive results, restore system state
9. **Final Report & Notification** - Deliver executive summary with clear status

**Key Features:**
- Parallel test execution with dependency management
- Automatic retry for flaky tests (max 2 retries)
- Regression detection (previously passing tests now failing)
- Flaky test identification and flagging
- GitHub integration: auto-comment on issues, update labels, close/reopen
- Multiple output formats: executive summary, detailed results, JUnit XML, JSON
- Performance tracking: slowest tests, trends vs. previous runs

**Usage:**
```
"Run all test suites"
"Verify fixes for issues #42, #45, #48"
"Test voice mode and WordPress integration"
"Run regression tests before deployment"
```

---

## Workflow Standards

All workflows follow these standards:

### Structure
- **ID Format:** `workflow-{timestamp}` (e.g., `workflow-1742785200001`)
- **Node ID Format:** `node-{timestamp}` (e.g., `node-1742785200001`)
- **Edge ID Format:** `edge-{timestamp}` (e.g., `edge-1742785200001`)
- **Version:** Semantic versioning (starts at 1)

### Node Configuration
Each agent node includes:
- `agentId`: Reference to agent definition
- `agentName`: Human-readable name
- `additionalPrompt`: Detailed instructions for the agent
- `orchestratorInstructions`: How to pass data between nodes
- `successCriteria`: Clear completion definition
- `completionContract`: Empty (reserved for future use)

### Error Handling
- Browser blocks: Retry with alternative search terms or wait + retry
- Rate limits: Exponential backoff (30s initial, 300s max)
- File write errors: Log and continue with fallback paths
- Test failures: Retry up to 2 times, capture diagnostics

### Logging
All workflows log to:
- `~/sulla/daily-logs/{date}/{workflow-name}/{run-id}.md`
- `~/sulla/logs/workflows/{workflow-name}.log`
- Master tracker files in project directories

### Output Locations
- **Lead Gen:** `~/sulla/projects/leads/{category}/`
- **Prospecting:** `~/sulla/projects/prospects/social-media/{industry}/`
- **Test Results:** `~/sulla/logs/test-results/{timestamp}/`

---

## Creating New Workflows

When creating new workflows:

1. **Check for duplicates:** `file_search query="workflow name"`
2. **Follow the schema:** Use existing workflows as templates
3. **Validate before saving:** `validate_sulla_workflow filePath="~/sulla/workflows/new-workflow.yaml"`
4. **Test execution:** Run manually first, then enable scheduling
5. **Document:** Add to this README with purpose, steps, and usage examples

---

## Maintenance

- Review workflow performance monthly
- Update agent prompts based on results
- Add error handling for new failure modes
- Prune unused workflows to `archive/` folder
- Keep this README current with all active workflows

---

*Last updated: 2026-03-21*
