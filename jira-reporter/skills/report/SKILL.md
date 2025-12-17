---
name: jira-report
description: Jira 보고서 생성. 담당자별/기간별/상태별/유형별/영역별 보고서, 팀별 이슈 현황, 진행률 분석 요청 시 사용.
---

# Jira 보고서

## 보고서 유형
1.담당자별 2.기간별 3.상태별 4.유형별 5.영역별
→ 상세: [REPORT_TYPES.md](REPORT_TYPES.md)

## 기간 형식
| 입력 | JQL |
|------|-----|
| 2025-1Q | updated>="2025-01-01" AND updated<"2025-04-01" |
| 2025-06 | updated>="2025-06-01" AND updated<"2025-07-01" |
| week | updated>=startOfDay(-7d) |
| 30d | updated>=startOfDay(-30d) |

⚠️ `yyyy`, `yyyy-MM-dd~yyyy-MM-dd` → 분기/월/week/30d 선택 안내

## 팀 구성
[TEAM_CONFIG.md](TEAM_CONFIG.md) 참조 (Site, Project, 팀원 설정)

## 공통 설정
```
JQL: project={TEAM_CONFIG.Project} AND assignee IN ({팀원들}) AND {기간}
fields: key,status,issuetype,summary
maxResults: 100, pagination 필수
저장: .tasks/reports/{기능}-{팀}-{기간}-{YYMMDD-HHmm}.md
```

## 영역 분류
[AREA_CLASSIFICATION.md](AREA_CLASSIFICATION.md) 참조