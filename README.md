# Claude Plugin Marketplace

Claude Code 플러그인 마켓플레이스입니다.

| 항목 | 내용 |
|------|------|
| 마켓플레이스명 | `jira-tools-marketplace` |
| 저장소 | `cooingpop/claude-plugin` |

---

## 설치 방법

### Step 1: 마켓플레이스 추가

```bash
/plugin marketplace add cooingpop/claude-plugin
```

### Step 2: 플러그인 설치

```bash
/plugin install {플러그인명}@jira-tools-marketplace --scope {스코프}
```

### 설치 스코프

| 스코프 | 저장 위치 | 적용 범위 | 용도 |
|--------|----------|----------|------|
| `user` | `~/.claude/plugins/` | 내 모든 프로젝트 | 개인적으로 항상 쓰는 플러그인 |
| `project` | `.claude/plugins/` (git tracked) | 이 repo의 모든 협업자 | 팀 전체가 공유할 플러그인 |
| `local` | `.claude/plugins/` (gitignored) | 이 repo에서 나만 | 실험용, 개인 커스텀용 |

### Step 3: 설치 확인

```bash
/plugin list
```

### 스킬 목록 확인

```
"사용 가능한 스킬 뭐있어?"
```

> 상세 설치 가이드: [docs/INSTALLATION_GUIDE.md](docs/INSTALLATION_GUIDE.md)

---

## 제공 플러그인

| 플러그인 | 설명 | 상세 |
|----------|------|------|
| `jira-reporter` | Jira 이슈 보고서 자동 생성 (담당자별/기간별/상태별/유형별/영역별) | [README](jira-reporter/README.md) |

---

## 프로젝트 구조

```
claude-plugin/
├── .claude-plugin/
│   └── marketplace.json           # 마켓플레이스 설정
│
├── {plugin-name}/                 # 플러그인 폴더
│   ├── .claude-plugin/
│   │   └── plugin.json            # 플러그인 설정
│   ├── skills/
│   │   └── {skill-name}/
│   │       └── SKILL.md           # 스킬 정의 (필수)
│   └── README.md                  # 플러그인 사용 가이드
│
├── docs/
│   └── INSTALLATION_GUIDE.md      # 상세 설치 가이드
│
└── README.md                      # 마켓플레이스 소개 (이 파일)
```

---

## 새 플러그인 추가 가이드

### 1. 플러그인 폴더 생성

```bash
mkdir -p {plugin-name}/.claude-plugin
mkdir -p {plugin-name}/skills/{skill-name}
```

### 2. plugin.json 작성

`{plugin-name}/.claude-plugin/plugin.json`:

```json
{
  "name": "{plugin-name}",
  "version": "1.0.0",
  "description": "플러그인 설명",
  "author": {
    "name": "작성자"
  }
}
```

### 3. SKILL.md 작성

`{plugin-name}/skills/{skill-name}/SKILL.md`:

```markdown
---
name: {skill-name}
description: 이 스킬이 언제 호출되어야 하는지 설명. Claude가 이 설명을 보고 자동으로 스킬을 선택합니다.
---

# 스킬 제목

## 동작 규칙
Claude가 수행할 작업을 명확히 작성...

## 출력 형식
결과물 형식 정의...
```

#### SKILL.md 필수 필드

| 필드 | 규칙 | 예시 |
|------|------|------|
| `name` | 소문자, 숫자, 하이픈만 (최대 64자) | `jira-report` |
| `description` | 호출 조건 설명 (최대 1024자) | `Jira 보고서 생성. 담당자별/기간별... 요청 시 사용.` |

> **중요**: `description`이 구체적일수록 Claude가 정확하게 스킬을 호출합니다.

### 4. 플러그인 README.md 작성

`{plugin-name}/README.md`:

플러그인 사용자를 위한 상세 가이드를 작성합니다.

```markdown
# {Plugin Name}

플러그인 설명...

## 제공 기능
...

## 사용 예시
...

## 초기 설정
...

## 문제 해결
...
```

### 5. marketplace.json에 플러그인 추가

`.claude-plugin/marketplace.json`의 `plugins` 배열에 추가:

```json
{
  "plugins": [
    {
      "name": "jira-reporter",
      "source": "./jira-reporter",
      "description": "기존 플러그인",
      "version": "1.0.0"
    },
    {
      "name": "{new-plugin}",
      "source": "./{new-plugin}",
      "description": "새 플러그인 설명",
      "version": "1.0.0"
    }
  ]
}
```

### 6. ROOT README.md 업데이트

이 파일의 **제공 플러그인** 테이블에 추가:

```markdown
| `{new-plugin}` | 간단한 설명 | [README]({new-plugin}/README.md) |
```

---

## 플러그인 작성 규칙

### 파일명 규칙

| 파일 | 위치 | 필수 |
|------|------|------|
| `plugin.json` | `{plugin}/.claude-plugin/plugin.json` | O |
| `SKILL.md` | `{plugin}/skills/{skill}/SKILL.md` | O |
| `README.md` | `{plugin}/README.md` | O |

### YAML frontmatter 규칙

- `---`로 시작하고 `---`로 끝내기
- 탭 사용 금지 (스페이스만 사용)

### description 작성 팁

- 어떤 요청에 반응해야 하는지 **키워드 포함**
- 예: "보고서", "분석", "현황", "요약", "생성" 등
- 구체적일수록 Claude가 정확하게 호출

---

## 기여 방법

### 브랜치 전략

```
main
  └── feature/{플러그인명}     # 새 플러그인 추가
  └── feature/{플러그인명}/{기능명}  # 기존 플러그인에 기능 추가
  └── fix/{플러그인명}/{수정내용}    # 버그 수정
  └── docs/{문서명}            # 문서 수정
```

### 커밋 메시지

```
feat({plugin}): 새 플러그인 추가
feat({plugin}): 새 스킬 추가 - {스킬명}
fix({plugin}): 버그 수정 - {내용}
docs: README 업데이트
docs({plugin}): 사용 가이드 업데이트
```

### PR 체크리스트

**새 플러그인 추가 시:**
- [ ] `{plugin}/.claude-plugin/plugin.json` 생성
- [ ] `{plugin}/skills/{skill}/SKILL.md` 생성
- [ ] `{plugin}/README.md` 생성
- [ ] `.claude-plugin/marketplace.json`에 플러그인 추가
- [ ] ROOT `README.md` 제공 플러그인 테이블에 추가
- [ ] 로컬 테스트 완료

**기존 플러그인 수정 시:**
- [ ] 버전 업데이트 (`plugin.json`, `marketplace.json`)
- [ ] 변경 이력 업데이트 (`{plugin}/README.md`)
- [ ] 로컬 테스트 완료

### 로컬 테스트 방법

```bash
# 프로젝트 폴더에서
/plugin marketplace add ./
/plugin install {plugin-name}@jira-tools-marketplace --scope local

# 테스트 후 제거
/plugin uninstall {plugin-name}@jira-tools-marketplace
```

---

## 문의

- 플러그인 버그/기능 요청: [GitHub Issues](https://github.com/cooingpop/claude-plugin/issues)
- 새 플러그인 기여: Pull Request

> *macOS/Linux 환경은 테스트되지 않았습니다. 문제 발생 시 이슈로 알려주세요.*
