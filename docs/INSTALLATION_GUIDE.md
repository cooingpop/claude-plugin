# Jira Reporter 플러그인 설치 가이드

Claude Code에서 Jira 보고서를 자동 생성하는 플러그인입니다.

---

## 플러그인 정보

| 항목 | 내용 |
|------|------|
| 플러그인명 | `jira-reporter` |
| 마켓플레이스 | `claude-tools-marketplace` |
| 저장소 | `cooingpop/claude-plugin` |
| 버전 | 1.0.0 |

---

## 설치 방법

### Step 1: 마켓플레이스 추가

```bash
/plugin marketplace add cooingpop/claude-plugin
```

### Step 2: 플러그인 설치

설치 스코프에 따라 명령어가 다릅니다.

#### 옵션 A: User Scope (개인 전역 설치) - 권장

**모든 프로젝트**에서 사용 가능합니다.

```bash
/plugin install jira-reporter@claude-tools-marketplace --scope user
```

| 항목 | 내용 |
|------|------|
| 저장 위치 | `~/.claude/plugins/` |
| 적용 범위 | 내 모든 프로젝트 |
| 용도 | 개인적으로 항상 쓰는 플러그인 |

#### 옵션 B: Project Scope (팀 공유 설치)

**이 프로젝트의 모든 협업자**가 사용 가능합니다. (Git에 포함됨)

```bash
/plugin install jira-reporter@claude-tools-marketplace --scope project
```

| 항목 | 내용 |
|------|------|
| 저장 위치 | `.claude/plugins/` (git tracked) |
| 적용 범위 | 이 repo의 모든 협업자 |
| 용도 | 팀 전체가 공유할 플러그인 |

#### 옵션 C: Local Scope (개인 로컬 설치)

**이 프로젝트에서 나만** 사용합니다. (Git에 포함 안 됨)

```bash
/plugin install jira-reporter@claude-tools-marketplace --scope local
```

| 항목 | 내용 |
|------|------|
| 저장 위치 | `.claude/plugins/` (gitignored) |
| 적용 범위 | 이 repo에서 나만 사용 |
| 용도 | 실험용, 개인 커스텀용 |

---

## 설치 확인

```bash
/plugin list
```

`jira-reporter`가 목록에 표시되면 설치 완료!

---

## 스킬 확인

Claude에게 자연어로 물어보세요:

```
"사용 가능한 스킬 뭐있어?"
```

---

## 사용 예시

설치 후 자연어로 요청하면 자동으로 동작합니다:

```
"backend 팀 이번 달 담당자별 보고서 만들어줘"

"payment 팀 2025-1Q 진행률 보여줘"

"최근 30일 기간별 이슈 추이 분석해줘"

"이번 주 상태별 현황 요약해줘"

"backend 팀 영역별로 이슈 분류해줘"
```

### 기간 형식

| 입력 | 의미 |
|------|------|
| `2025-1Q` | 2025년 1분기 |
| `2025-06` | 2025년 6월 |
| `week` | 최근 7일 |
| `30d` | 최근 30일 |

---

## 초기 설정 (중요!)

플러그인 설치 후, **팀 정보를 설정**해야 합니다.

플러그인이 설치된 위치에서 `skills/report/TEAM_CONFIG.md` 파일을 수정하세요:

```markdown
# 팀 구성

Site: your-company.atlassian.net | Project: YOUR_PROJECT | 기본팀: backend

## 팀원
backend: 홍길동,김철수
payment: 이영희,박민수
messaging: 최지훈,정수연
...
```

### 설정 파일 위치

| 스코프 | 파일 경로 |
|--------|----------|
| User | `~/.claude/plugins/jira-reporter/skills/report/TEAM_CONFIG.md` |
| Project | `.claude/plugins/jira-reporter/skills/report/TEAM_CONFIG.md` |
| Local | `.claude/plugins/jira-reporter/skills/report/TEAM_CONFIG.md` |

---

## 제공 보고서 유형

| 유형 | 설명 |
|------|------|
| 담당자별 | 팀원별 이슈 처리 현황, 완료율 |
| 기간별 | 주차/월별 이슈 발생/완료 추이 |
| 상태별 | 진행중/완료/대기 등 상태 분포 |
| 유형별 | Bug/Task/Story 등 유형별 분류 |
| 영역별 | 태그/키워드 기반 영역 분류 |

---

## 플러그인 제거

```bash
/plugin uninstall jira-reporter@claude-tools-marketplace
```

---

## 플러그인 업데이트

```bash
/plugin uninstall jira-reporter@claude-tools-marketplace
/plugin install jira-reporter@claude-tools-marketplace --scope user
```

---

## 문제 해결

### 스킬이 인식되지 않을 때

1. Claude Code 재시작
2. `/plugin list`로 설치 확인
3. 스킬 확인: "사용 가능한 스킬 뭐있어?"

### 보고서가 생성되지 않을 때

1. `TEAM_CONFIG.md` 설정 확인
2. Atlassian MCP 서버 연결 확인
3. Jira 프로젝트 접근 권한 확인

---

## 문의

문제 발생 시 GitHub 이슈에 남겨주세요.

> *macOS/Linux 환경은 테스트되지 않았습니다.*
