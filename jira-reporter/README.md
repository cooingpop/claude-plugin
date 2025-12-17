# Jira Reporter

Jira 이슈를 분석하여 다양한 형식의 보고서를 자동 생성하는 Claude Code 플러그인입니다.

| 항목 | 내용 |
|------|------|
| 플러그인명 | `jira-reporter` |
| 버전 | 1.0.0 |
| 스킬 | `jira-report` |

---

## 제공 기능

| 보고서 유형 | 설명 |
|-------------|------|
| 담당자별 | 팀원별 이슈 처리 현황, 완료율 |
| 기간별 | 주차/월별 이슈 발생/완료 추이 |
| 상태별 | 진행중/완료/대기 등 상태 분포 |
| 유형별 | Bug/Task/Story 등 유형별 분류 |
| 영역별 | 태그/키워드 기반 영역 분류 |

---

## 사용 예시

자연어로 요청하면 자동으로 동작합니다:

```
"backend 팀 이번 달 담당자별 보고서 만들어줘"

"payment 팀 2025-1Q 진행률 보여줘"

"최근 30일 기간별 이슈 추이 분석해줘"

"이번 주 상태별 현황 요약해줘"

"backend 팀 영역별로 이슈 분류해줘"
```

---

## 기간 형식

| 입력 | 의미 | JQL 변환 |
|------|------|----------|
| `2025-1Q` | 2025년 1분기 | `updated>="2025-01-01" AND updated<"2025-04-01"` |
| `2025-06` | 2025년 6월 | `updated>="2025-06-01" AND updated<"2025-07-01"` |
| `week` | 최근 7일 | `updated>=startOfDay(-7d)` |
| `30d` | 최근 30일 | `updated>=startOfDay(-30d)` |

---

## 초기 설정 (필수)

플러그인 설치 후 `skills/report/TEAM_CONFIG.md` 파일을 본인 환경에 맞게 수정하세요.

### 설정 파일 위치

| 설치 스코프 | 파일 경로 |
|------------|----------|
| User | `~/.claude/plugins/jira-reporter/skills/report/TEAM_CONFIG.md` |
| Project | `.claude/plugins/jira-reporter/skills/report/TEAM_CONFIG.md` |
| Local | `.claude/plugins/jira-reporter/skills/report/TEAM_CONFIG.md` |

### 설정 예시

```markdown
# 팀 구성

Site: your-company.atlassian.net | Project: YOUR_PROJECT | 기본팀: backend

## 팀원
backend: 홍길동,김철수
payment: 이영희,박민수
messaging: 최지훈,정수연
community: 강동원,한지민
ios: 이준호,김태희
android: 송중기,전지현
```

### 설정 항목 설명

| 항목 | 설명 | 예시 |
|------|------|------|
| Site | Atlassian 사이트 주소 | `your-company.atlassian.net` |
| Project | Jira 프로젝트 키 | `YOUR_PROJECT` |
| 기본팀 | 팀 미지정 시 사용할 기본 팀 | `backend` |
| 팀원 | `팀명: 이름1,이름2` 형식 | `backend: 홍길동,김철수` |

---

## 폴더 구조

```
jira-reporter/
├── .claude-plugin/
│   └── plugin.json           # 플러그인 메타데이터
├── skills/
│   └── report/
│       ├── SKILL.md          # 스킬 정의
│       ├── TEAM_CONFIG.md    # 팀/프로젝트 설정 (수정 필요)
│       ├── REPORT_TYPES.md   # 보고서 형식 정의
│       └── AREA_CLASSIFICATION.md  # 영역 분류 규칙
└── README.md
```

### 파일 설명

| 파일 | 역할 |
|------|------|
| `SKILL.md` | 스킬 이름, 설명, 동작 규칙 |
| `TEAM_CONFIG.md` | Atlassian 사이트, 프로젝트, 팀원 정보 **(수정 필요)** |
| `REPORT_TYPES.md` | 5가지 보고서 출력 형식 정의 |
| `AREA_CLASSIFICATION.md` | 이슈 영역 분류 규칙 (태그, 키워드) |

---

## 요구사항

- Claude Code 설치
- Atlassian MCP 서버 연결 ([설정 방법](https://docs.anthropic.com/en/docs/claude-code))
- Jira 프로젝트 접근 권한

---

## 문제 해결

### 스킬이 인식되지 않을 때

1. Claude Code 재시작
2. `/plugin list`로 설치 확인
3. "사용 가능한 스킬 뭐있어?" 로 확인

### 보고서가 생성되지 않을 때

1. `TEAM_CONFIG.md` 설정 확인
2. Atlassian MCP 서버 연결 상태 확인
3. Jira 프로젝트 접근 권한 확인
4. 팀원 이름이 Jira assignee와 일치하는지 확인

### 특정 팀원 이슈가 조회되지 않을 때

- `TEAM_CONFIG.md`의 팀원 이름이 Jira의 assignee 표시 이름과 정확히 일치해야 함
- 대소문자, 공백 주의

---

## 변경 이력

| 버전 | 날짜 | 변경 내용 |
|------|------|----------|
| 1.0.0 | 2025-01 | 최초 릴리즈 |

---

## 문의

문제 발생 시 [GitHub 이슈](https://github.com/cooingpop/claude-plugin/issues)에 남겨주세요.

> *macOS/Linux 환경은 테스트되지 않았습니다.*
