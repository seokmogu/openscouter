# AGENTS.md - OpenScouter Workspace

## Every Session

1. Read `MEMORY.md` — long-term memory
2. Read `memory/YYYY-MM-DD.md` (today + yesterday) — recent context
3. Check `positions/` — active job openings
4. Check `candidates/` — current pipeline

## Directory Structure

```
workspace/
├── MEMORY.md              # Long-term memory
├── SOUL.md                # Your persona
├── AGENTS.md              # This file
├── TOOLS.md               # Platform-specific notes
├── positions/             # Job descriptions
│   └── senior-react-dev.md
├── candidates/            # Candidate profiles
│   ├── by-position/       # Grouped by JD
│   └── pool/              # General talent pool
├── outreach/              # Draft messages
│   └── templates/         # Reusable templates
└── memory/                # Daily logs
    └── YYYY-MM-DD.md
```

## Candidate Profile Format

When saving a candidate, use this structure:

```markdown
# [Name] — [Primary Role]

## Source

- Found via: [Google/GitHub/LinkedIn/Blog/etc]
- Profile: [URLs]
- Date found: YYYY-MM-DD

## Summary

[2-3 sentence overview]

## Skills & Experience

- **Primary:** [main tech stack]
- **Years:** [experience level]
- **Location:** [city/remote]
- **Current:** [current company/role if known]

## Evidence

- [GitHub repos, blog posts, talks, contributions]
- [Quality indicators — stars, activity, writing depth]

## JD Match Score

- **Position:** [which JD]
- **Score:** [1-10]
- **Strengths:** [why they fit]
- **Gaps:** [what's missing]

## Status

- [ ] Found
- [ ] Evaluated
- [ ] Outreach drafted
- [ ] Contacted
- [ ] Responded
- [ ] Interview scheduled
```

## Search Strategy

When asked to find candidates:

1. **Understand the JD** — extract must-haves vs nice-to-haves
2. **Multi-source search:**
   - Google: `"senior react developer" site:github.com OR site:medium.com OR site:velog.io Seoul`
   - GitHub: search by language, location, contribution activity
   - Tech blogs: velog.io, medium.com, tistory.com, dev.to
   - LinkedIn/사람인/원티드: via browser (user's logged-in session)
3. **Evaluate** — don't just collect names, assess quality
4. **Score & rank** — against the JD requirements
5. **Report** — concise summary with top candidates

## Browser Usage (Critical)

You access recruitment platforms via the user's browser. Rules:

- **Never** try to log in for the user — use existing sessions
- **Ask** before navigating to a new platform
- **Be efficient** — don't browse aimlessly, have a search plan
- **Save** what you find to local files immediately
- Platforms: LinkedIn, 사람인, 원티드, 잡코리아, 로켓펀치

## Persistent Search Mode

"계속 찾아줘" / "멈출 때까지" / "최대한 많이" 라고 하면:

1. `persistent-search` 스킬을 읽고 따른다
2. 라운드별로 다른 소스를 검색하며 계속 돌아간다
3. 매 라운드마다 진행 보고한다
4. "그만" / "멈춰" / "stop" 이라고 하면 멈춘다
5. 검색 상태는 `memory/search-state.json`에 저장한다

## Safety

- Store candidate data locally only (workspace files)
- Don't send candidate info to external services
- Don't contact candidates without explicit approval
- PII handling: store only what's needed for recruitment
- `trash` > `rm`
