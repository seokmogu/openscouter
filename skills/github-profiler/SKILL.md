---
name: github-profiler
description: "Deep analysis of GitHub profiles for candidate evaluation. Fetches contribution data, repo quality, tech stack, and activity patterns using GitHub API and web scraping."
metadata: { "openclaw": { "emoji": "🐙" } }
---

# GitHub Profiler

Analyze GitHub profiles to evaluate developer candidates.

## Trigger

User provides a GitHub username or URL for evaluation, or talent-search needs deeper GitHub analysis.

## Process

### Step 1: Fetch Profile

```bash
# Public API (no auth needed for basic info)
curl -s https://api.github.com/users/[username]
```

Or via `web_fetch`:

```
web_fetch: https://github.com/[username]
```

**Extract:**

- Name, bio, location, company
- Public repos count, followers/following
- Blog/website URL
- Created date (account age)

### Step 2: Top Repos

```bash
curl -s "https://api.github.com/users/[username]/repos?sort=stars&per_page=10"
```

**For each repo evaluate:**

- ⭐ Stars — community recognition
- 🍴 Forks — others building on it
- 📝 README quality — communication skill
- 🧪 Tests present? — engineering discipline
- 🔄 Last updated — still active?
- 📋 Issues managed? — project management
- 🏷️ Topics/tags — self-categorization

### Step 3: Contribution Analysis

```
web_fetch: https://github.com/[username]
```

From the profile page:

- **Contribution graph:** green density in last year
- **Contribution count:** total in last year
- **Streak patterns:** consistent vs burst
- **Pinned repos:** what they're proud of

### Step 4: Language Breakdown

```bash
# Per-repo languages
curl -s https://api.github.com/repos/[username]/[repo]/languages
```

Aggregate across repos to get:

- Primary language(s)
- Language diversity
- Match with JD requirements

### Step 5: Collaboration Signals

Check for:

- PRs to other repos (open source contributor?)
- Issues filed on popular projects
- Code review comments
- Organization memberships

```
web_fetch: https://github.com/[username]?tab=organizations
```

### Step 6: Code Quality Spot Check

Pick their most starred repo and `web_fetch` a source file:

- Code organization
- Naming conventions
- Error handling
- Comments/documentation

### Step 7: Output

```markdown
## GitHub Profile: [username]

**Name:** [name] | **Location:** [location] | **Company:** [company]

### Activity

- Account age: [years]
- Last year contributions: [count]
- Activity pattern: [consistent/sporadic/burst]

### Tech Stack

- Primary: [languages with %]
- Frameworks: [detected from repos]

### Top Repos

1. [repo] ⭐[stars] — [description]
2. [repo] ⭐[stars] — [description]

### Quality Signals

- README quality: [high/medium/low]
- Testing: [yes/no/partial]
- CI/CD: [yes/no]
- Code style: [notes]

### Open Source

- Contributions to others: [yes/no, notable ones]
- Orgs: [list]

### Overall: [1-10] / 10

[2-3 sentence summary]
```

## GitHub API Rate Limits

- Unauthenticated: 60 requests/hour
- Be efficient: batch what you can
- Cache results in candidate files
- If rate limited, use `web_fetch` on the HTML profile instead

## Red Flags

- Empty profile, no repos → not a GitHub user, check elsewhere
- Only forked repos, no original work → learner stage
- No activity in 1+ year → may not be actively coding
- All repos are tutorial follow-alongs → junior signal
