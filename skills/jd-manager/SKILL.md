---
name: jd-manager
description: "Create, manage, and refine job descriptions. Structures requirements, suggests market-appropriate terms, and saves to the positions directory."
metadata:
  { "openclaw": { "emoji": "📋" } }
---

# JD Manager

Create and manage job descriptions.

## Trigger

User asks to create/write/update a JD, job posting, or position description.

## Process

### Step 1: Gather Info

Ask (or extract from conversation):
- **Title:** what's the role called?
- **Team/dept:** where does this sit?
- **Must-haves:** non-negotiable skills/experience
- **Nice-to-haves:** bonus qualifications
- **Location:** office/hybrid/remote, city
- **Seniority:** junior/mid/senior/lead
- **Salary range:** if available

### Step 2: Create JD File

Save to `positions/[slug].md`:

```markdown
# [Job Title]

**Status:** Open
**Created:** YYYY-MM-DD
**Location:** [city/remote]
**Seniority:** [level]
**Salary:** [range if known]

## Overview
[2-3 sentence role summary]

## Must-Have Requirements
- [ ] [requirement 1]
- [ ] [requirement 2]

## Nice-to-Have
- [ ] [bonus 1]
- [ ] [bonus 2]

## Tech Stack
- [primary technologies]

## Responsibilities
- [key duty 1]
- [key duty 2]

## Search Keywords
- English: [search terms]
- Korean: [검색 키워드]
- GitHub: [languages, topics]

## Pipeline
| Candidate | Score | Status | Date |
|-----------|-------|--------|------|
| (empty)   |       |        |      |

## Notes
[any additional context]
```

### Step 3: Generate Search Keywords

Auto-generate search terms for talent-search skill:
- English variations of the role
- Korean equivalents
- GitHub language/topic tags
- Related technologies

## Tips
- Keep must-haves to 3-5 items (more = fewer candidates)
- "3+ years React" is better than "5+ years JavaScript frameworks"
- Include team size and reporting structure if known
- Add salary range — saves time filtering
