---
name: talent-search
description: "Multi-source talent discovery via Google dorking, GitHub profiling, and tech blog analysis. Finds candidates from public sources — no paid recruiter tools needed. Uses browser for logged-in platforms when available."
metadata: { "openclaw": { "emoji": "🔍" } }
---

# Talent Search

Find candidates from public sources using Google dorking, GitHub analysis, and blog crawling.

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

### Step 2: Google Dorking (Primary)

This is the main discovery engine. Use `web_search` with Boolean search techniques:

**이력서/포트폴리오 직접 찾기:**

```
"이력서" OR "resume" OR "portfolio" "[skill]" "[location]" filetype:pdf
"about me" "[skill]" developer "[location]"
```

**LinkedIn 공개 프로필:**

```
site:linkedin.com/in "[skill] developer" "[location]"
site:linkedin.com/in "[기술]" "[도시]" "경력"
```

**GitHub 프로필:**

```
site:github.com "[skill]" "[location]" followers
site:github.com/[username] — 개별 프로필 심층 분석
```

**기술 블로그에서 전문가 찾기:**

```
site:velog.io "[skill]" "아키텍처" OR "설계" OR "최적화" OR "성능"
site:tistory.com "[skill]" "시니어" OR "리드" OR "경험"
site:medium.com "[skill]" "[location]" developer
site:dev.to "[skill]" "[location]"
```

**컨퍼런스 발표자:**

```
"[skill]" "발표" OR "speaker" OR "세미나" "[location]" site:youtube.com OR site:slideshare.net
"[skill]" "if kakao" OR "deview" OR "feconf" OR "woowacon"
```

**Tip:** 검색어를 영어/한국어 양쪽으로 돌려라. 같은 사람이 다른 언어로 다른 플랫폼에 있을 수 있다.

### Step 3: GitHub Deep Dive

유망한 GitHub 프로필 발견 시 `web_fetch`로 상세 분석:

```
web_fetch: https://github.com/[username]
```

**체크 항목:**

- **Contribution graph:** 최근 6개월 활동?
- **Pinned repos:** 기술스택 확인, 코드 품질
- **Stars received:** 커뮤니티 인정
- **Bio/README:** 위치, 현재 소속, 관심사
- **Organizations:** 소속 회사/팀
- **Languages:** 주 사용 언어 비율

**GitHub 사용자 검색 직접:**

```
web_fetch: https://github.com/search?q=location%3Aseoul+language%3Atypescript+followers%3A%3E5&type=users
```

**코드 품질 판단 기준:**

- README 작성 수준 (설명, 설치법, 예시)
- 테스트 존재 여부
- CI/CD 설정
- 커밋 메시지 품질
- 이슈/PR 관리

### Step 4: 블로그 콘텐츠 분석

발견한 블로그에서 `web_fetch`로 2-3개 글 읽기:

**평가 기준:**
| 항목 | 시니어 신호 | 주니어 신호 |
|------|-----------|-----------|
| 주제 | 아키텍처, 설계 결정, 트레이드오프 | 튜토리얼, 설치법, 기초 |
| 깊이 | 왜 그렇게 했는지, 대안 비교 | 어떻게 하는지만 |
| 빈도 | 꾸준히 (월 1-2회+) | 산발적 |
| 경험 | 실제 프로덕션 사례 | 개인 프로젝트/학습 |

### Step 5: Stack Overflow / 커뮤니티

```
site:stackoverflow.com/users "[skill]" "[location]"
```

- 답변 수, reputation, 태그 전문성 확인

### Step 6: 플랫폼 검색 (보조, 로그인 있을 때만)

브라우저에 로그인된 세션이 있으면 추가 검색:

**LinkedIn (무료 계정):**

```
browser → linkedin.com/search/results/people
→ keyword + location 필터
→ 상위 결과 프로필 확인
```

⚠️ 무료는 월 검색 제한 있음. 핵심 후보만 확인.

**로켓펀치:**

```
browser → rocketpunch.com/people
→ 기술스택 + 지역 필터
→ 프로필 비교적 열린 편
```

**접근 불가 시:** 에러 내지 말고 "이 플랫폼은 로그인이 필요합니다. 건너뛸게요."로 처리.

### Step 7: 교차 검증 & 프로필 통합

같은 사람이 여러 소스에서 발견될 수 있음:

- GitHub username → Google → LinkedIn → Blog
- Blog author name → GitHub → LinkedIn

**통합 프로필 작성:**
하나의 후보 파일에 모든 소스 정보를 합침.

### Step 8: 저장 & 보고

후보별 파일 생성: `candidates/pool/[name]-[role].md`
AGENTS.md의 프로필 템플릿 사용.

**보고 형식:**

```
🔍 [Role] 검색 결과

[N]명 발견 (소스: Google, GitHub, velog 등)

1. **[이름]** ⭐ 8/10
   - [주요 기술] | [경력 수준] | [위치]
   - 발견: GitHub(활발), velog(아키텍처 글 다수)
   - 특이사항: [눈에 띄는 점]

2. **[이름]** ⭐ 7/10
   ...

📁 candidates/pool/ 에 저장 완료
다음: 상세 평가? 아웃리치 초안?
```

## 검색 키워드 치트시트

### 직군별 한국어 키워드

| 직군       | 키워드                                 |
| ---------- | -------------------------------------- |
| 프론트엔드 | 프론트엔드, FE, 웹개발, UI개발         |
| 백엔드     | 백엔드, BE, 서버개발, API              |
| 풀스택     | 풀스택, 웹개발자                       |
| 데이터     | 데이터엔지니어, 데이터분석, ML엔지니어 |
| DevOps     | DevOps, SRE, 인프라, 클라우드          |
| 모바일     | iOS, Android, Flutter, React Native    |
| PM         | PM, PO, 프로덕트매니저                 |
| 디자이너   | UI/UX, 프로덕트디자이너                |

### 시니어 신호 키워드

```
"리드" OR "테크리드" OR "아키텍트" OR "시니어"
"설계" OR "아키텍처" OR "의사결정" OR "트레이드오프"
"팀" OR "멘토링" OR "코드리뷰" OR "온보딩"
```

### 한국 테크 컨퍼런스 (발표자 = 상급자)

- if(kakao), Deview (NAVER), FEConf, WoowaCon
- PyCon Korea, JSConf Korea, Spring Camp
- NDC (게임), SOSCON (삼성)

## 주의사항

- 한 번에 너무 많이 검색하지 말 것 (Google rate limit)
- 검색 간 2-3초 간격 유지
- LinkedIn은 무료 계정 제한 있으므로 아껴 쓸 것
- 개인정보는 공개된 것만 수집 (비공개 이메일/전화번호 X)
- 후보 파일에는 공개 URL만 저장
