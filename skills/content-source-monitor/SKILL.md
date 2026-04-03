# Content Source Monitor Skill

This skill defines the data sources to monitor for open source project content ideas and the scoring criteria for evaluating viral potential.

## Monitored Sources

### Primary Sources (Parallel Agents)

| Source | URL | Strategy | Output File |
|--------|-----|----------|-------------|
| GitHub Trending | https://github.com/trending | Daily trending repos + ICP-based keyword search | `github-ideas.md` |
| opensourceprojects.dev | https://opensourceprojects.dev | Featured/curated open source projects | `opensourceprojects-dev-ideas.md` |
| Reddit | r/opensource, r/github, r/IndieHack, r/selfhosted | Top posts from last 7 days | `reddit-ideas.md` |
| X.com | #opensource, #github, #devtools | Recent posts with engagement | `twitter-ideas.md` |

### Extended Sources (Dynamic Dispatch)

| Source | URL | Strategy |
|--------|-----|----------|
| Hacker News | https://news.ycombinator.com | Top posts with 100+ points |
| Product Hunt | https://producthunt.com | Top software/tools of the day |
| Gitee Trending | https://gitee.com/trending | Chinese open source trending |
| LibreStars | https://libre.star | Curated open source directory |

## ICP Context (Loaded from Marketing Project)

The orchestrator loads ICP from `~/sulla/projects/marketing-plan/icp.md` or similar. The ICP defines:

- **Audience**: Who we're writing for (developers, indie hackers, solopreneurs)
- **Pain Points**: Problems they face
- **Topics of Interest**: AI, automation, devtools, self-hosting, SaaS building
- **Goals**: What they're trying to achieve

This ICP is passed to each agent so they can:
- Search for GitHub repos using relevant keywords
- Filter Reddit/Reddit posts that match audience needs
- Craft hooks that resonate with this specific crowd

## Source Requirements

Each source agent MUST:

1. **Scan** their target source for open source projects
2. **Filter** to only projects that match audience (using ICP context)
3. **Format** each idea as:

```markdown
## [Project Name]
**Source:** [where found]
**URL:** [project link]
**Description:** [2-3 sentence description]
**Hook:** [1-2 sentence pitch - why should readers care? What's the curiosity gap?]
**Tags:** [comma-separated: ai, devtools, automation, saas, etc.]
```

## Viral Scoring Criteria

Each idea is scored 1-10 on:

| Criterion | Weight | Questions |
|-----------|--------|-----------|
| **Universal Appeal** | 20% | Does it solve a problem almost every developer faces? |
| **Audience Fit** | 20% | Does it directly help indie hackers/solopreneurs build faster? |
| **Lead Gen Potential** | 20% | Could this warm audience to want Merchant Protocol services? |
| **Curiosity Gap** | 15% | Is there a "wait, what?" element that makes people click? |
| **Actionability** | 15% | Can reader implement/use this TODAY? |
| **Viral Hook** | 10% | Does it have share-worthy angle (controversy, impressive, funny)? |

**Minimum Score to Proceed: 7/10**

## Deduplication Rules

Before scoring, remove:

1. **Already Covered**: Projects mentioned on merchantprotocol.com in last 90 days
2. **Duplicate Topics**: Same project found on multiple sources (keep first found)
3. **Expired Trends**: Projects from >30 days ago
4. **Non-Open Source**: Proprietary/SaaS-only tools (must be MIT/Apache/GPL/BSD licensed)

## Output Locations

- Individual source files: `~/sulla/daily-logs/{YYYY-MM-DD}/content-discovery/{source}-ideas.md`
- Filtered ideas: `~/sulla/daily-logs/{YYYY-MM-DD}/content-discovery/filtered-ideas.md`
- Scored rankings: `~/sulla/daily-logs/{YYYY-MM-DD}/content-discovery/scored-ideas.md`
- Top 10-20: `~/sulla/daily-logs/{YYYY-MM-DD}/content-discovery/daily-top-ideas.md`

## Skill Usage

This skill is loaded by the Content Discovery Pipeline to:
1. Know WHERE to find project ideas
2. Know HOW to format each idea (with hook)
3. Know HOW to score each idea
4. Know WHERE to write outputs

The workflow then dispatches parallel agents with ICP context to execute against these sources.
