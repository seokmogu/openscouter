---
name: persistent-search
description: "Continuous talent search that keeps finding candidates until the user says stop. Runs in rounds — each round searches different sources/queries, saves results, reports progress, then continues. Use when the user wants to maximize candidate discovery."
metadata: { "openclaw": { "emoji": "🔄" } }
---

# Persistent Search

Continuous candidate discovery that runs until stopped.

## Trigger

User says things like:

- "계속 찾아줘" / "멈출 때까지 찾아줘"
- "최대한 많이" / "계속 돌려"
- "persistent search" / "keep searching"

## Architecture

```
Round 1: Google dorking (이력서, 포트폴리오)
  → 결과 저장 → 보고 → 10초 대기
Round 2: GitHub 유저 검색 (location + language)
  → 결과 저장 → 보고 → 10초 대기
Round 3: 기술 블로그 (velog, tistory)
  → 결과 저장 → 보고 → 10초 대기
Round 4: 컨퍼런스 발표자 (FEConf, Deview, etc)
  → 결과 저장 → 보고 → 10초 대기
Round 5: 회사 기술블로그 (토스, 네이버, 카카오, 배민 등)
  → 결과 저장 → 보고 → 10초 대기
Round 6: Stack Overflow + 커뮤니티
  → 결과 저장 → 보고 → 10초 대기
Round 7: Google dorking 변형 (다른 키워드 조합)
  → ... 반복
```

## Process

### Step 0: Initialize Search State

Create or read `memory/search-state.json`:

```json
{
  "active": true,
  "position": "senior-react-dev",
  "query": "시니어 React 개발자 서울",
  "round": 0,
  "totalFound": 0,
  "foundNames": [],
  "startedAt": "2026-02-28T12:00:00Z",
  "lastReportAt": null,
  "sources": {
    "google-resume": false,
    "google-portfolio": false,
    "google-linkedin": false,
    "github-users": false,
    "github-repos": false,
    "velog": false,
    "tistory": false,
    "medium": false,
    "feconf": false,
    "deview": false,
    "woowacon": false,
    "if-kakao": false,
    "toss-blog": false,
    "naver-d2": false,
    "kakao-blog": false,
    "woowa-blog": false,
    "line-blog": false,
    "banksalad-blog": false,
    "stackoverflow": false,
    "google-dorking-v2": false,
    "google-dorking-v3": false,
    "dev-to": false,
    "hashnode": false,
    "speakerdeck": false,
    "youtube-talks": false
  }
}
```

### Step 1: Pick Next Source

Read `memory/search-state.json`.
Find the first source with `false`. Execute that source's search strategy.

### Step 2: Search (per source)

Each source has its own search approach. Use `web_search` or `web_fetch` or `exec(curl)`.

**Deduplication is critical:** before saving, check `foundNames` array. Skip duplicates.

### Step 3: Save Results

For each NEW candidate found:

1. Save to `candidates/pool/[name]-[role].md`
2. Add name to `foundNames` array in search-state.json
3. Increment `totalFound`

### Step 4: Report Progress

After each round, send a brief update:

```
🔄 Persistent Search — Round [N]
Source: [source name]
New: [X]명 발견 | Total: [Y]명
Latest: [name1], [name2], ...
Status: 계속 검색 중... ("그만" 하면 멈춤)
```

### Step 5: Rate Limit Handling

Between rounds, wait to avoid rate limits:

- Google: 15초 간격
- GitHub API: 10초 간격
- 기타: 5초 간격

If rate limited:

- Skip to next source type
- Mark current source as partially done
- Come back later

### Step 6: Continue or Stop

After reporting, check:

- Did user say "stop", "그만", "멈춰", "enough"? → Stop
- All sources exhausted? → Generate new query variations and continue
- Otherwise → next round

### Step 7: When All Sources Done

Generate **variation queries** and add new sources:

```
Original: "시니어 react 개발자 서울"
Variation 1: "프론트엔드 리드" "react" "서울"
Variation 2: "tech lead" "frontend" "seoul" site:linkedin.com/in
Variation 3: "react" "아키텍처" "설계" 블로그
Variation 4: specific company searches (쿠팡, 당근마켓, 야놀자...)
```

Add these as new sources and continue the loop.

### Step 8: Final Report (on stop)

When user stops the search:

```
🔍 Persistent Search 완료

총 검색 시간: [duration]
총 라운드: [rounds]
총 발견: [N]명

소스별:
- Google: [n]명
- GitHub: [n]명
- 블로그: [n]명
- 컨퍼런스: [n]명
- 기술블로그: [n]명

전체 후보 목록: candidates/pool/
```

## Source Search Strategies

### google-resume

```
web_search: "이력서" "[skill]" "[location]" filetype:pdf
web_search: "resume" "[skill]" "[location]"
```

### google-portfolio

```
web_search: "portfolio" "[skill]" developer "[location]"
web_search: "about me" "[skill]" "[location]" developer
```

### google-linkedin

```
web_search: site:linkedin.com/in "[job title]" "[location]"
```

### github-users

```
web_fetch: https://github.com/search?q=location%3A[location]+language%3A[lang]+followers%3A%3E10&type=users
```

### github-repos

```
web_fetch: https://github.com/search?q=[skill]+[location]&type=repositories&s=stars
→ repo 주인 프로필 확인
```

### velog

```
web_search: site:velog.io "[skill]" ("아키텍처" OR "설계" OR "회고" OR "경험")
```

### tistory

```
web_search: site:tistory.com "[skill]" ("시니어" OR "리드" OR "아키텍처")
```

### feconf

```
web_search: "FEConf" "[year]" "발표" OR "speaker" "[skill]"
web_fetch: https://feconf.kr (speaker list)
```

### deview

```
web_search: "DEVIEW" "[year]" "[skill]" site:deview.kr OR site:d2.naver.com
```

### Company tech blogs

```
web_search: site:toss.tech "[skill]"
web_search: site:d2.naver.com "[skill]"
web_search: site:tech.kakao.com "[skill]"
web_search: site:techblog.woowahan.com "[skill]"
web_search: site:engineering.linecorp.com "[skill]"
web_search: site:blog.banksalad.com "[skill]"
```

## Stop Commands

Recognize these as stop signals:

- "그만" / "멈춰" / "스톱" / "stop" / "enough"
- "됐어" / "충분해" / "이만" / "끝"
- Any message while search is running that indicates stop intent

On stop: set `active: false` in search-state.json, generate final report.

## Rules

1. **Never skip deduplication** — check foundNames before every save
2. **Always report after each round** — user needs to see progress
3. **Respect rate limits** — wait between requests, skip if blocked
4. **Save immediately** — don't batch saves, write each candidate right away
5. **Vary queries** — don't repeat the same search twice
6. **Track everything** — update search-state.json after every round
