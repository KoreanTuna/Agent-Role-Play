# Quick Start Guide

Agent Role Play 프로젝트를 5분 안에 시작하는 가이드입니다.

## 🎯 목표

이 가이드를 따라하면:
- ✅ 에이전트 역할 이해
- ✅ 첫 업무 지시서 작성
- ✅ 협업 스레드 생성
- ✅ 결과 보고서 작성

## 📚 시작 전 필수 읽기

**모든 에이전트 공통**
1. [README.md](../README.md) - 프로젝트 개요
2. [docs/README.md](README.md) - 문서 구조
3. [용어 사전](glossary.md) - 핵심 용어 정의

**역할별 필독**
- **PM**: [roles/pm.md](roles/pm.md) → [career_map_plan_doc.md](career_map_plan_doc.md)
- **프론트엔드**: [roles/frontend.md](roles/frontend.md) → [career_map_story_board.md](career_map_story_board.md)
- **서버**: [roles/server.md](roles/server.md) → [career_map_erd.md](career_map_erd.md)
- **문서화**: [roles/documentation.md](roles/documentation.md) → [developer-guide.md](developer-guide.md)

읽는 순서: 역할 정의 → 기획 문서 → 운영 가이드

## 🚀 5분 시작하기

### 0단계: docs 서브모듈 최신화(코드 레포에서, 30초)

```bash
# 코드 레포 루트에서 실행
git submodule update --init --recursive
git submodule update --remote docs
git submodule status  # 상태 확인
```

### 1단계: 역할 확인 (1분)

당신의 역할에 맞는 문서를 읽으세요.

```bash
# PM이라면
cat docs/roles/pm.md

# 프론트엔드라면
cat docs/roles/frontend.md

# 서버라면
cat docs/roles/server.md

# 문서화라면
cat docs/roles/documentation.md
```

### 2단계: 프로젝트 맥락 파악 (2분)

**CareerMap 서비스 핵심**
- 이력 기반 직장 추천
- 동료 네트워크 분석
- 이직 가능성 예측

**현재 단계**: MVP 개발 중

**주요 기능**:
1. 회원가입/로그인
2. 이력 입력
3. 직장 추천
4. 동료 네트워크
5. 이직 예측

자세한 내용: [career_map_plan_doc.md](career_map_plan_doc.md)

### 3단계: 첫 작업 시작 (2분)

#### PM 에이전트인 경우

1. 업무 지시서 작성
```bash
# 스레드 파일 생성
touch docs/threads/$(date +%Y-%m-%d)-my-first-task.md
```

2. 템플릿 복사 및 작성
```markdown
# [지시] 프론트엔드 - 로그인 화면 구현

## 기본 정보
- 요청자: PM 에이전트
- 담당 에이전트: 프론트엔드
- 요청 일자: 2025-12-19
- 목표 완료 일자: 2025-12-26
- 우선순위: 높음

## 배경/목적
사용자가 이메일/비밀번호로 로그인할 수 있는 화면 구현

## 요구 사항
- 이메일 입력 필드 (형식 검증)
- 비밀번호 입력 필드 (마스킹)
- 로그인 버튼
- 회원가입 링크
- 비밀번호 재설정 링크

## 범위
- 포함: UI 구현, 입력 검증, API 연동
- 제외: 소셜 로그인 (2차 개발)

## 산출물
- PR 링크
- 스크린샷 (성공/실패 상태)
- 테스트 결과

## 완료 조건
- [ ] 입력 검증 동작
- [ ] API 연동 성공
- [ ] 에러 처리 완료
- [ ] 테스트 통과
```

#### 프론트엔드/서버 에이전트인 경우

1. 지시서 확인
```bash
# 스레드 파일에서 [지시] 섹션 확인
grep -A 20 "\[지시\]" docs/threads/*.md
```

2. 작업 시작 → 진행 업데이트
```markdown
## [업데이트] 2025-12-19

- 진행: 로그인 UI 기본 레이아웃 완료
- 차단: 디자인 가이드 필요 (색상/폰트)
- 다음 액션: PM에게 디자인 에셋 요청
```

3. 작업 완료 → 결과 보고
```markdown
## [결과] 프론트엔드 - 로그인 화면 구현 완료

### 기본 정보
- 담당 에이전트: 프론트엔드
- 작업 기간: 2025-12-19 ~ 2025-12-25
- 관련 지시서: docs/threads/2025-12-19-login-screen.md

### 작업 요약
이메일/비밀번호 로그인 화면 구현 완료

### 상세 변경 내용
- UI: lib/presentation/auth/login_screen.dart
- 상태관리: lib/presentation/auth/login_controller.dart
- API 연동: lib/data/api/auth_api.dart

### 테스트/검증 결과
- ✅ 입력 검증 동작 확인
- ✅ API 연동 테스트 통과
- ✅ 에러 처리 확인

### 산출물
- PR: https://github.com/repo/pull/123
- 스크린샷: docs/assets/login-screen.png

### 이슈 및 리스크
- 없음

### 다음 작업 제안
- 회원가입 화면 구현
- 비밀번호 재설정 플로우
```

#### 문서화 에이전트인 경우

1. 모든 스레드 파일 검토
```bash
ls -lt docs/threads/
```

2. 용어/절차 일관성 확인

3. 문서 갱신
- 불일치 발견 시 해당 스레드에 `[이슈]` 섹션 추가
- 용어 사전 업데이트
- README 인덱스 갱신

## 📁 파일 구조 이해하기

```
docs/
├── README.md                      # 문서 구조 인덱스
├── glossary.md                    # 용어 사전 ⭐️
├── quick-start.md                 # 이 파일
├── developer-guide.md             # 운영 가이드
├── career_map_plan_doc.md         # 기획서
├── career_map_story_board.md      # 화면 정의
├── career_map_erd.md              # API/DB 설계
├── roles/                         # 역할 정의
│   ├── pm.md
│   ├── frontend.md
│   ├── server.md
│   └── documentation.md
├── templates/                     # 문서 템플릿
│   ├── task-brief.md
│   ├── task-report.md
│   ├── issue-log.md
│   ├── decision-record.md
│   └── plan.md
└── threads/                       # 실제 협업 기록 ⭐️
    └── YYYY-MM-DD-주제.md
```

## 🔄 일반적인 작업 흐름

```
1. PM: 업무 지시서 작성 (task-brief 템플릿)
   ↓
2. 담당자: 지시서 확인 → 작업 시작
   ↓
3. 담당자: 진행 중 업데이트 (매일 또는 필요 시)
   ↓
4. 담당자: 이슈 발생 시 issue-log로 공유
   ↓
5. 담당자: 작업 완료 → 결과 보고서 작성 (task-report)
   ↓
6. PM/문서화: 검수 → 승인/보완 요청 (decision-record)
   ↓
7. 다음 작업으로 진행
```

## 📝 자주 사용하는 명령어

### 새 스레드 생성
```bash
# 오늘 날짜로 스레드 파일 생성
touch docs/threads/$(date +%Y-%m-%d)-작업주제.md
```

### 템플릿 복사
```bash
# 업무 지시서 템플릿 복사
cat docs/templates/task-brief.md > docs/threads/$(date +%Y-%m-%d)-new-task.md
```

### 스레드 검색
```bash
# 특정 키워드가 포함된 스레드 찾기
grep -r "로그인" docs/threads/

# 최근 수정된 스레드 확인
ls -lt docs/threads/ | head -5
```

## ✅ 체크리스트

작업을 시작하기 전에 확인하세요:

- [ ] 내 역할 정의 문서를 읽었다
- [ ] 프로젝트 기획서(plan_doc/story_board/erd)를 확인했다
- [ ] 용어 사전을 읽었다
- [ ] 스레드 파일 네이밍 규칙을 이해했다 (YYYY-MM-DD-주제.md)
- [ ] 필요한 템플릿 위치를 알고 있다 (docs/templates/)
- [ ] 협업 규칙을 이해했다 (문서 기반, 시간순 기록)

## 🆘 도움이 필요할 때

1. **용어가 헷갈릴 때**: [glossary.md](glossary.md) 참고
2. **절차가 불명확할 때**: [developer-guide.md](developer-guide.md) 참고
3. **템플릿 작성 예시가 필요할 때**: `docs/templates/examples/` 참고
4. **차단 이슈 발생 시**: 스레드에 `[이슈]` 섹션 추가 후 @PM 호출

## 🎓 다음 단계

Quick Start를 완료했다면:

1. [developer-guide.md](developer-guide.md) 전체 읽기
2. 서브모듈 설정 (코드 레포 연동)
3. CI/CD 파이프라인 이해
4. 첫 PR 생성 및 리뷰 프로세스 경험

## 📌 핵심 원칙 (다시 한번)

1. **문서 우선**: 모든 것을 문서로 기록
2. **단일 책임**: 각 업무는 하나의 담당자
3. **추적 가능성**: 지시 → 작업 → 결과 → 결정을 한 스레드에
4. **짧은 주기 공유**: 매일 상태 업데이트

---

**문서 업데이트**: 2025-12-19
**담당**: 문서화 에이전트
