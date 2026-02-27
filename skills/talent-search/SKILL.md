---
name: talent-search
description: "Multi-source talent discovery. Searches Google, GitHub, tech blogs, LinkedIn, and job platforms to find candidates matching a job description. Uses browser automation for logged-in platforms."
metadata:
  { "openclaw": { "emoji": "🔍" } }
---

# Talent Search

Find candidates across multiple sources for a given job description or criteria.

## Trigger

User asks to find/search/source candidates, developers, engineers, designers, etc.

## Process

### Step 1: Parse Requirements
Extract from user request:
- **Role:** job title / function
- **Skills:** required tech stack, tools, languages
- **Experience:** years, seniority level
- **Location:** city, country, remote OK?
- **Nice-to-haves:** bonus skills, industry experience

If a JD file exists in `positions/`, read it for full context.

### Step 2: Google Search
Use `web_search` with targeted queries:

```
"senior react developer" (Seoul OR 서울) site:github.com
"react" "3 years" developer blog (velog.io OR medium.com OR tistory.com)
"react developer" portfolio site:linkedin.com/in
```

Vary queries — try English and Korean terms.

### Step 3: GitHub Discovery
Use `web_search` or `web_fetch` on GitHub:

```
web_search: "site:github.com react typescript seoul followers:>10"
web_fetch: https://github.com/search?q=location%3Aseoul+language%3Atypescript&type=users
```

For promising profiles, fetch their GitHub page to check:
- Contribution activity (green squares)
- Popular repos (stars, forks)
- Tech stack from pinned repos
- README quality

### Step 4: Blog & Content Search
Search tech blog platforms:
- velog.io (Korean dev community)
- tistory.com (Korean blogs)
- medium.com
- dev.to
- Personal blogs found via Google

Evaluate:
- Writing quality and depth
- Technical expertise level
- Recency of posts
- Topic relevance to the role

### Step 5: Platform Search (Browser)
If user has logged-in sessions, use `browser` tool:

**LinkedIn:**
```
browser → navigate to linkedin.com/search
→ enter search criteria
→ snapshot results
→ extract profile info
```

**사람인/원티드/잡코리아/로켓펀치:**
```
browser → navigate to platform search
→ apply filters
→ extract candidate list
```

⚠️ Only use browser for platforms where user is logged in.
Ask before accessing a new platform for the first time.

### Step 6: Save & Report
For each candidate found:
1. Create file: `candidates/pool/[name]-[role].md` using the AGENTS.md template
2. Score against JD requirements (1-10)
3. Note source and evidence

Report summary to user:
```
🔍 Talent Search Results: [Role]

Found [N] candidates across [sources]:

1. **김OO** (Score: 8/10)
   - React 5년, TypeScript, Next.js
   - GitHub: 활발한 오픈소스 기여
   - Source: GitHub + velog

2. **이OO** (Score: 7/10)
   ...

Files saved to candidates/pool/
Next: evaluate in detail? draft outreach?
```

## Tips
- Search both Korean (한국어) and English terms
- GitHub contribution graphs say more than listed skills
- Blog post quality > quantity
- LinkedIn "Open to Work" badge = higher response rate
- Check if candidates have recent activity (stale profiles = likely not looking)
