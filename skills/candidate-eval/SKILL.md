---
name: candidate-eval
description: "Evaluate and score candidates against job descriptions. Analyzes GitHub activity, blog posts, portfolios, and profile data. Produces structured assessments with match scores."
metadata:
  { "openclaw": { "emoji": "📊" } }
---

# Candidate Evaluation

Deep-dive evaluation of a candidate against a specific job description.

## Trigger

User asks to evaluate, assess, score, or compare candidates.

## Process

### Step 1: Load Context
- Read the JD from `positions/[role].md`
- Read the candidate profile from `candidates/`
- If profile is thin, search for more info first (use talent-search skill)

### Step 2: GitHub Analysis
If candidate has a GitHub profile, use `web_fetch`:

Check:
- **Activity:** commits in last 6 months?
- **Languages:** match JD requirements?
- **Quality:** code organization, README quality, testing
- **Collaboration:** PRs to other repos, issue discussions
- **Stars:** community recognition

### Step 3: Content Analysis
If candidate has blog/portfolio:
- Read 2-3 recent posts via `web_fetch`
- Assess: technical depth, communication skill, domain relevance
- Note any talks, open source work, or community involvement

### Step 4: Score

Rate on 5 dimensions (each 1-10):

| Dimension | Weight | Score | Notes |
|-----------|--------|-------|-------|
| Technical Skills | 30% | ? | Stack match, depth |
| Experience Level | 25% | ? | Years, seniority |
| Evidence Quality | 20% | ? | GitHub, blog, portfolio |
| Culture Signals | 15% | ? | Communication, collaboration |
| Availability | 10% | ? | Open to Work, recent activity |

**Overall = weighted average**

### Step 5: Update & Report

Update the candidate file with evaluation results.

Report:
```
📊 Candidate Evaluation: [Name]
Position: [Role]

Overall Score: [X]/10

✅ Strengths:
- [strength 1]
- [strength 2]

⚠️ Concerns:
- [concern 1]

📝 Recommendation: [Strong fit / Worth interviewing / Pass]
```

## Comparison Mode

When asked to compare candidates:
- Load all candidate profiles for the position
- Score each on the same dimensions
- Rank and present as a table
- Recommend top N for outreach
