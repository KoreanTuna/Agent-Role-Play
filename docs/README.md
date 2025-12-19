# Agent Role 기반 업무 자동화 문서 구조

이 디렉터리는 PM, 프론트엔드, 서버, 문서화 에이전트가 **문서 기반으로 협업**할 수 있도록
표준 템플릿과 역할 정의를 제공합니다.

## 🚀 빠른 시작
- **처음이신가요?** → [Quick Start 가이드](quick-start.md) (5분 소요)
- **용어가 헷갈리시나요?** → [용어 사전](glossary.md)
- **운영 방법이 궁금하신가요?** → [개발자용 운영 가이드](developer-guide.md)

## 1) 역할 정의
- [PM 에이전트](roles/pm.md)
- [프론트엔드 에이전트](roles/frontend.md)
- [서버 에이전트](roles/server.md)
- [문서화 에이전트](roles/documentation.md)

## 2) 공통 문서 템플릿
- [업무 지시서 템플릿](templates/task-brief.md) | [작성 예시](templates/examples/task-brief-example.md)
- [업무 결과 보고 템플릿](templates/task-report.md) | [작성 예시](templates/examples/task-report-example.md)
- [이슈/리스크 기록 템플릿](templates/issue-log.md) | [작성 예시](templates/examples/issue-log-example.md)
- [의사결정 기록 템플릿](templates/decision-record.md) | [작성 예시](templates/examples/decision-record-example.md)
- [주간/스프린트 계획 템플릿](templates/plan.md) | [작성 예시](templates/examples/plan-example.md)

## 3) 문서 기반 소통 규칙
- 모든 업무 요청은 **업무 지시서 템플릿**으로 시작한다.
- 각 에이전트는 작업 종료 시 **업무 결과 보고 템플릿**을 남긴다.
- 장애/리스크/질문은 **이슈/리스크 기록 템플릿**을 통해 공유한다.
- 합의된 결론은 **의사결정 기록 템플릿**으로 확정한다.

## 4) 문서 저장 위치(권장)
```
docs/
  quick-start.md         # 빠른 시작 가이드 ⭐️
  glossary.md            # 용어 사전 ⭐️
  developer-guide.md     # 개발자용 운영 가이드
  career_map_plan_doc.md      # CareerMap 기획서
  career_map_story_board.md   # 화면/UX 정의
  career_map_erd.md           # API/DB 설계
  roles/                 # 각 에이전트 역할 정의
    pm.md
    frontend.md
    server.md
    documentation.md
  templates/             # 공통 템플릿 모음
    task-brief.md
    task-report.md
    issue-log.md
    decision-record.md
    plan.md
    examples/            # 템플릿 작성 예시 ⭐️
      task-brief-example.md
      task-report-example.md
      issue-log-example.md
      decision-record-example.md
      plan-example.md
  threads/               # 실제 협업 기록 (날짜/이슈별로 파일 생성)
    YYYY-MM-DD-주제.md
```

> 실제 협업 기록은 `docs/threads/`에 `YYYY-MM-DD-주제.md` 형식으로 파일을 생성하여 관리합니다.
