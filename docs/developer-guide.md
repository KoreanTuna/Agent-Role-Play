# Agent Role Play 운영 가이드 (개발자용)

네 가지 에이전트(PM, 프론트엔드, 서버, 문서화)가 문서 기반으로 매끄럽게 협업하도록 업무 지시·검수 방식과 커뮤니케이션 규칙을 정의합니다.

## 빠른 시작

- **처음 시작하시나요?** → [Quick Start 가이드](quick-start.md)를 먼저 읽어보세요 (5분 소요)
- **용어가 헷갈리시나요?** → [용어 사전](glossary.md)을 참고하세요
- **템플릿 작성 예시가 필요하신가요?** → [templates/examples/](templates/examples/)를 확인하세요

## 1. 기본 원칙

- 문서 우선: 모든 요청·결정·리스크를 템플릿 기반 문서로 남긴다(`docs/templates` 활용).
- 단일 책임자: 각 업무는 하나의 책임 에이전트가 오너가 된다(공동 작업 시 역할을 명시).
- 추적 가능성: 지시 → 작업 기록 → 결과 보고 → 의사결정이 한 스레드로 연결되도록 `docs/threads/`에 기록한다.
- 짧은 주기 공유: 상태/차단 요소를 짧게라도 하루 1회 남긴다(stand-up 템플릿 권장).

## 2. 에이전트 역할 요약

- PM: 업무 지시/우선순위, 의존성 정리, 완료 승인.
- 프론트엔드: UI/UX 구현, 인터랙션·상태관리, 프론트 테스트.
- 서버: API·데이터모델 설계/구현, 배포/운영 체크.
- 문서화: 산출물 구조화, 가이드/릴리스 노트 작성, 기록 정합성 확인.

## 3. 업무 진행 흐름 (문서 기준)

1. 개시: PM이 `docs/templates/task-brief.md`로 지시서 작성 → `docs/threads/YYYY-MM-DD-주제.md`에 붙여넣고 오너/마감/우선순위/완료조건 정의.
2. 실행: 각 에이전트는 질문·리스크를 `docs/templates/issue-log.md` 포맷으로 같은 스레드에 추가, 의사결정은 `decision-record.md` 포맷으로 기록.
3. 결과: 작업자는 `task-report.md`로 결과 보고서를 같은 스레드에 남기고, 관련 산출물 링크를 명시.
4. 검수: PM/문서화 에이전트가 체크리스트로 승인 여부 판단 후, 결정문서(`decision-record`)에 “승인/보완요청” 기록.
5. 후속: 필요 시 다음 작업 지시서를 같은 스레드 하위 섹션에 생성하거나 새 스레드를 연다(주제 단위로 스레드를 가볍게 유지).

## 4. 지시서 작성 규격 (요청 시 꼭 채우기)

`task-brief` 템플릿을 사용하며, 최소 항목:

- 목표/배경: 왜 하는지, 성공 정의(숫자/사용자 시나리오).
- Scope In/Out: 포함·제외 범위, 선행/후행 작업.
- 요구사항: 기능/UX/성능/보안 등 명시적 요구.
- 산출물 & 완료조건: 파일/PR/데모/문서 형태와 품질 기준.
- 우선순위/마감: 절대 마감일, 중간 점검일.
- 의존성/제약: API, 디자인, 계정, 환경 등.
- 테스트 기대: 필수 테스트 리스트(수동/자동), 수용 기준.
- 리뷰어/승인자: 코드/문서 검토 담당 명시.

예시 지시 블록(threads 파일에 붙여넣기):

```
## [지시] FE - 프로필 카드 1차
- 목표: 사용자 대시보드에 프로필 카드 추가, 모바일 360px 이상 대응.
- Scope: In - 이름/직함/현재 회사/배지; Out - 편집 기능.
- 산출물: PR 링크, 스토리북 스토리 1개, 스냅샷 테스트.
- 마감/체크인: 6/12 EOD, 중간 점검 6/11 오전.
- 의존성: API GET /profile v1 확정 필요(서버팀).
- 테스트: 모바일/데스크톱 크로스브라우저 수동 체크, Jest 스냅샷.
- 리뷰어: 서버(계약 필드 확인), 문서화(용어/텍스트 톤).
```

## 5. 검수/리뷰 체크리스트

공통(모든 결과 보고서 기준):

- 요구사항-산출물 매핑: 지시서 항목별로 결과 보고서에 대응 링크가 있는가.
- 품질 근거: 테스트 결과/스크린샷/로그 등이 증빙으로 포함됐는가.
- 리스크 처리: 오픈 이슈가 `issue-log`로 정리되고 차단 여부가 명확한가.
- 결정 기록: 중요한 선택지가 `decision-record`로 남았는가.

프론트엔드 검수:

- UI 일관성: 디자인 스펙/반응형/접근성(포커스, 키보드, 대체 텍스트) 확인.
- 상태/오류: 빈 상태, 로딩, 실패 UI가 구현·테스트됐는가.
- 빌드/테스트: linters/test 통과 증빙, 스토리북/스크린샷 링크.

서버 검수:

- 계약 준수: API 스펙, 응답 스키마, 에러 코드가 지시서와 일치하는가.
- 안정성: 로그/메트릭 포인트, 실패 재시도·타임아웃 등 회복력 확인.
- 보안/성능: 인증/인가, 입력 검증, N+1/캐싱 여부, 마이그레이션 절차.

문서화 검수:

- 최신성: 결과 보고서, README/가이드, 결정 기록 간 내용이 일치하는가.
- 재현성: 로컬 실행/배포 절차가 누락 없이 따라 할 수 있는가.
- 톤/용어: 서비스 표준 용어/스타일 가이드에 맞는가.

## 6. 커뮤니케이션 규칙 (스레드 운용)

- 파일 위치: 모든 협업 기록은 `docs/threads/날짜-주제.md`에 시간 순으로 append.
- 업데이트 형식: `[날짜] 진행/차단/다음 액션` 3줄로 짧게 남긴다.
- 질문 우선순위: 차단 이슈는 `issue-log`로 남기고 @오너 호출, 기타는 댓글 섹션에 묶는다.
- 승인 라벨: 스레드에 “승인됨/보완필요/블록됨” 텍스트 라벨을 남겨 상태를 명확히 한다.

## 7. 폴더/파일 네이밍 가이드

- threads 파일: `YYYY-MM-DD-핵심주제.md` (예: `2024-06-10-profile-card.md`)
- 섹션: `[지시]`, `[업데이트]`, `[결과]`, `[결정]`, `[이슈]` 접두어 사용으로 스캔 용이성 확보.
- 산출물 링크: 코드(PR/커밋), 디자인, 데이터, 로그, 데모 URL을 결과 보고서에 모두 기입.

## 8. 코드 레포 연동(서브모듈 전략)

문서/운영 레포(이 레포)를 단일 소스로 두고, 서버/플러터 코드 레포에 `docs/` 경로로 서브모듈을 연결합니다.

### A) 초기 설정(레포별 1회)

```
# 코드 레포 루트에서
git submodule add <ops-docs-repo-url> docs
git submodule update --init --recursive
git commit -am "chore: add ops-docs submodule"
```

- 클론 시: `git clone --recurse-submodules <code-repo-url>`
- 기존 클론: `git submodule update --init --recursive`

### B) 작업 플로우(에이전트 공통)

- 문서 생성/수정은 항상 서브모듈 루트(`docs/threads/...`, `docs/templates/...`)에서 커밋 → `ops-docs` 브랜치로 PR.
- 코드 PR 설명에 반드시 해당 스레드 경로/링크를 적는다(누락 시 PR 템플릿이 체크).
- 문서 머지 후 코드 레포 서브모듈 포인터 업데이트:

```bash
git submodule update --remote docs
git add docs
git commit -m "chore: bump docs"
```

**검증 절차**:
```bash
# 1. 서브모듈이 올바르게 연결되었는지 확인
git submodule status

# 2. 서브모듈 내용 확인
cd docs && git log -1 --oneline

# 3. 서브모듈 최신 상태 확인
git submodule update --remote --merge
```

### C) PR 템플릿 예시(코드 레포 `.github/pull_request_template.md`)

```
## Docs
- [ ] docs 스레드 링크: docs/threads/2024-06-11-feature-x.md
```

### D) CI 세팅(코드 레포)

- GitHub Actions: `actions/checkout@v4`에 `submodules: true` 옵션 추가로 docs 폴더 포함.
- 문서 빌드/테스트는 돌리지 말고(속도 저하), 스레드 링크 필수 확인만 유지.

### E) 운용 팁

- 권한: 모든 에이전트가 ops-docs 레포에 동일한 쓰기 권한을 가진다.
- 분리 배포: 스토어 심사 지연 등과 무관하게 문서 기록은 ops-docs에서만 이어간다.
- 네이밍: 스레드 파일명/섹션 접두어 규칙은 7장과 동일하게 유지.

## 9.1 PM → 클라이언트 질의 작성 규칙

- 클라이언트에게 확인이 필요한 질문/결정/자료 요청은 `docs/templates/client-question.md` 템플릿으로 작성한다.
- 작성 후 해당 스레드(`docs/threads/...`)에 붙여넣고 회신 기한/전달 경로를 명시한다.
- 회신을 받으면 같은 스레드에 “답변 요약”을 채워서 추적성을 유지한다.

## 9. 에이전트별 업무 시작 프롬프트

부트스트랩 파일(`*.md`)을 먼저 열어 맥락을 파악하고, 필요 시 같은 md에 진행 로그/질문을 append합니다.

- PM 에이전트
  - "PM 에이전트로서 업무 시작. 부트스트랩 파일은 \*.md 이므로 이 내용부터 확인 시작. 필요 시 md에 프롬프팅 내용 추가."
- 프론트엔드 에이전트
  - "프론트엔드 에이전트로서 업무 시작. 부트스트랩 파일은 \*.md 이므로 이 내용부터 확인 후 UI/UX/상태 흐름을 정리. 필요 시 md에 진행 로그와 질문을 추가."
- 서버 에이전트
  - "서버 에이전트로서 업무 시작. 부트스트랩 파일은 \*.md 이므로 이 내용부터 확인 후 API/데이터모델/배포 고려사항을 정리. 필요 시 md에 진행 로그와 차단 이슈를 추가."
- 문서화 에이전트
  - "문서화 에이전트로서 업무 시작. 부트스트랩 파일은 \*.md 이므로 이 내용부터 확인 후 용어/절차/재현성 관점에서 보완점을 기록. 필요 시 md에 편집/추가 내용을 남김."

## 10. 에이전트별 필독 md 파일

### 공통(모든 에이전트) - 읽기 순서
1. `docs/quick-start.md`: **가장 먼저 읽기** (5분 안에 시작)
2. `docs/glossary.md`: 용어 사전 (참고용, 필요 시)
3. `docs/README.md`: 문서 구조와 규칙 인덱스
4. `docs/templates/*.md`: 지시서/보고/이슈/결정 템플릿
5. `docs/templates/examples/*.md`: 템플릿 작성 예시
6. `docs/developer-guide.md`: 운영/서브모듈/프롬프트 규칙 (이 문서)
7. `docs/threads/*.md`: 실시간 진행/결정/결과 기록 (담당 스레드 열람/작성 필수)

### PM - 읽기 순서
1. `docs/roles/pm.md`: **역할 정의 (먼저 읽기)**
2. `docs/career_map_plan_doc.md`: 현재 계획/마일스톤/우선순위
3. `docs/career_map_story_board.md`: 사용자 시나리오/주요 UX 흐름
4. `docs/career_map_erd.md`: API/DB 설계 (서버와 협업 시 참고)

### 프론트엔드 - 읽기 순서
1. `docs/roles/frontend.md`: **역할 정의 (먼저 읽기)**
2. `docs/career_map_story_board.md`: 화면 흐름/UX 요구
3. `docs/career_map_plan_doc.md`: 범위/우선순위 및 마감
4. `docs/career_map_erd.md`: API 명세 (서버 연동 시 참고)

### 서버 - 읽기 순서
1. `docs/roles/server.md`: **역할 정의 (먼저 읽기)**
2. `docs/career_map_erd.md`: 데이터 모델/관계 정의, API 명세
3. `docs/career_map_plan_doc.md`: API 범위/우선순위
4. `docs/career_map_story_board.md`: 프론트엔드 요구사항 (연동 시 참고)

### 문서화 - 읽기 순서
1. `docs/roles/documentation.md`: **역할 정의 (먼저 읽기)**
2. `docs/career_map_plan_doc.md`: 전달해야 할 계획/스코프
3. `docs/career_map_story_board.md`: 사용자 흐름을 기반으로 용어/설명 정합성 체크
4. `docs/career_map_erd.md`: 데이터 용어/정의 일관성 검수
5. `docs/threads/*.md`: **모든 스레드 검토** (일관성 확인)

## 11. 기술 스택 버전 정보

### 프론트엔드 (Flutter)
- **Flutter SDK**: 3.16.0 이상 (권장: 최신 stable)
- **Dart**: 3.2.0 이상 (Flutter에 포함)
- **주요 패키지** (최소 버전):
  - `flutter_hooks`: ^0.20.0
  - `hooks_riverpod`: ^2.4.0
  - `dio`: ^5.0.0
  - `retrofit`: ^4.0.0
  - `go_router`: ^13.0.0
  - `get_it`: ^7.6.0
  - `injectable`: ^2.3.0
  - `flutter_secure_storage`: ^9.0.0

**호환성**:
- Android: API 21 (Android 5.0) 이상
- iOS: 12.0 이상
- Web: Chrome/Safari/Firefox 최신 2개 버전

### 서버 (Kotlin & Spring Boot)
- **Kotlin**: 1.9.0 이상
- **JDK**: 17 (LTS)
- **Spring Boot**: 3.2.0 이상
- **주요 의존성**:
  - Spring Web
  - Spring Data JPA
  - Spring Security
  - JWT (io.jsonwebtoken:jjwt)
  - PostgreSQL Driver (또는 H2 for dev)

**호환성**:
- 데이터베이스: PostgreSQL 14 이상 (프로덕션), H2 (개발)
- 빌드 툴: Gradle 8.0 이상

### 개발 환경
- **Git**: 2.30 이상 (서브모듈 지원)
- **IDE**:
  - Flutter: Android Studio / VS Code + Flutter 확장
  - Kotlin: IntelliJ IDEA / Android Studio
- **테스트**: Flutter Test, JUnit 5

### 업데이트 정책
- **Flutter**: 매 stable 릴리스마다 검토, 6개월마다 메이저 업데이트
- **Spring Boot**: 매 마이너 버전 검토, 1년마다 메이저 업데이트
- **의존성 보안 업데이트**: 즉시 적용

**버전 관리**:
- 프론트엔드: `pubspec.yaml`
- 서버: `build.gradle.kts`
- 문서화 에이전트가 분기마다 버전 호환성 체크
