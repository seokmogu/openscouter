---
name: outreach
description: "Draft personalized recruitment outreach messages for candidates. Generates LinkedIn messages, emails, or platform DMs tailored to each candidate's background and the position."
metadata:
  { "openclaw": { "emoji": "✉️" } }
---

# Outreach

Draft personalized recruitment messages for candidates.

## Trigger

User asks to write/draft/send outreach, message, or contact a candidate.

## Process

### Step 1: Load Context
- Read candidate profile from `candidates/`
- Read JD from `positions/`
- Check `outreach/templates/` for any existing templates

### Step 2: Personalize

Key personalization points:
- **Name** — obviously
- **Their work** — mention a specific repo, blog post, or project
- **Why them** — what specifically caught your eye
- **The role** — brief, compelling pitch
- **Call to action** — clear next step

### Step 3: Draft by Platform

**LinkedIn Message (300 char ideal, 1900 max):**
```
안녕하세요 [Name]님,

[Their specific project/blog post] 인상깊게 봤습니다.
현재 [Company]에서 [Role] 포지션을 찾고 있는데,
[Name]님의 [specific skill/experience]이 딱 맞을 것 같아 연락드립니다.

간단히 이야기 나눠볼 수 있을까요?
```

**Email (longer form OK):**
```
Subject: [Company] [Role] — [personalized hook]

[Name]님 안녕하세요,

[How you found them + specific compliment]
[Brief role description]
[Why they'd be a good fit — specific reasons]
[Call to action]

감사합니다,
[Recruiter name]
```

### Step 4: Save

Save draft to `outreach/[candidate-name]-[platform].md`

### Step 5: Get Approval

⚠️ **NEVER send messages without explicit user approval.**

Show draft and ask:
```
✉️ Outreach Draft: [Name] via [Platform]

[draft content]

Send this? Or want me to adjust?
```

## Templates

Store reusable templates in `outreach/templates/`:
- `linkedin-initial.md` — first contact
- `linkedin-followup.md` — no response follow-up
- `email-initial.md` — email first contact
- `referral-ask.md` — asking for referrals

## Tips
- Short > long for initial contact
- Mention something specific (not "I saw your profile")
- Korean for Korean candidates, English for international
- Morning sends get better response rates
- Follow up once after 5-7 days, then stop
