---
name: google-dorking
description: "Advanced Google search (dorking) for talent discovery. Generates and executes Boolean search queries to find developers, designers, and professionals from public web sources — resumes, portfolios, blogs, GitHub, conference talks."
metadata: { "openclaw": { "emoji": "🎯" } }
---

# Google Dorking for Talent

Specialized Boolean search queries for finding people on the public web.

## Trigger

User asks to search Google for candidates, or talent-search skill needs deeper Google coverage.

## Query Templates

### 이력서 / Resume

```
"이력서" "[skill]" "[location]" filetype:pdf
"resume" "[skill]" "[location]" filetype:pdf
"curriculum vitae" "[skill]" filetype:pdf site:drive.google.com
"portfolio" "[skill]" "[location]"
```

### GitHub 프로필

```
site:github.com "[location]" "[language]" followers
site:github.com "[username]" — 특정 유저 프로필
"github.com" "[skill]" developer "[location]" -jobs -hiring
```

### 기술 블로그 (한국)

```
site:velog.io "@[username]" "[skill]"
site:velog.io "[skill]" ("설계" OR "아키텍처" OR "회고")
site:tistory.com "[skill]" ("경험" OR "후기" OR "프로젝트")
site:brunch.co.kr "[skill]" "개발자" ("이직" OR "커리어")
```

### 기술 블로그 (글로벌)

```
site:medium.com "[skill]" developer "[location]"
site:dev.to "[skill]" "[location]"
site:hashnode.dev "[skill]" "[location]"
```

### LinkedIn 공개 프로필

```
site:linkedin.com/in "[job title]" "[location]"
site:linkedin.com/in "[skill]" "[company]" "[location]"
site:linkedin.com/posts "[skill]" "[location]" — 활동적인 사용자
```

### 컨퍼런스 발표자

```
"[conference name]" "speaker" OR "발표자" "[skill]" "[year]"
site:youtube.com "[conference]" "[skill]" "[year]"
site:speakerdeck.com "[skill]" "[location]"
site:slideshare.net "[skill]" "[location]"
```

### Stack Overflow

```
site:stackoverflow.com/users "[location]" "[skill]"
```

### 회사 기술 블로그 (해당 회사 출신 찾기)

```
site:tech.kakao.com "[skill]"
site:d2.naver.com "[skill]"
site:techblog.woowahan.com "[skill]"
site:engineering.linecorp.com "[skill]"
site:toss.tech "[skill]"
site:blog.banksalad.com "[skill]"
```

### 오픈소스 기여자

```
site:github.com/[project]/graphs/contributors — 주요 기여자 목록
site:github.com "[project]" "pull request" author:"[username]"
```

## 고급 연산자

| 연산자      | 용도        | 예시                      |
| ----------- | ----------- | ------------------------- |
| `site:`     | 특정 도메인 | `site:github.com`         |
| `filetype:` | 파일 형식   | `filetype:pdf`            |
| `"exact"`   | 정확한 문구 | `"senior developer"`      |
| `OR`        | 또는        | `"서울" OR "Seoul"`       |
| `-`         | 제외        | `-jobs -hiring -채용공고` |
| `intitle:`  | 제목에 포함 | `intitle:"이력서"`        |
| `inurl:`    | URL에 포함  | `inurl:portfolio`         |
| `after:`    | 날짜 이후   | `after:2024-01-01`        |

## 노이즈 제거 팁

채용공고가 아닌 **사람**을 찾으려면 노이즈 제거가 중요:

```
# 채용공고 제외
"[skill]" "[location]" -채용 -공고 -모집 -hiring -jobs -vacancy

# 교육/강의 제외
"[skill]" -강의 -tutorial -course -udemy

# 실제 개발자만
"[skill]" ("만들었" OR "구현" OR "개발했" OR "built" OR "shipped")
```

## 실행 규칙

1. 한 번에 3-5개 쿼리 실행 (rate limit)
2. 결과에서 **사람 이름 + URL** 추출
3. 같은 사람이 여러 소스에서 나오면 통합
4. 유망한 후보는 즉시 `candidates/pool/`에 저장
5. 쿼리와 결과를 `memory/YYYY-MM-DD.md`에 기록 (재검색 방지)
