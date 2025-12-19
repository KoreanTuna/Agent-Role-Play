# 2025-12-19 CareerMap Server 스레드

## 1. 오늘 착수한 작업
- docs 최신화 상태 확인 (`git status`: origin/main과 동기화, 변경 없음)
- ERD/플랜/스토리보드 스캔 후 서버 백로그 초안 작성
  - `server-status.md`: 현재 서버 레포 상태 요약
  - `server-next-steps.md`: MVP 기준 서버 백로그 및 우선순위 정리

## 2. 블로킹 이슈 / 확인 사항
1. 서버 스택/런타임 확정
   - 현재 레포는 Kotlin + Spring Boot 스켈레톤으로 추정되나, 실제 운영 타깃(DB: Postgres vs MySQL, 런칭 환경: Cloud Run/ECS 등) 미확정.
   - 영향: DB 선택 및 마이그레이션 전략, 프로파일 구성(local/dev/prod) 설계에 영향.
   - 제안 옵션
     - Option A: 로컬/H2 + prod/Postgres 전제 후 설계
     - Option B: 처음부터 Postgres 전제(local docker-compose 사용)
   - 결정 필요: 인프라/PM 측 우선순위 및 제약사항.

2. 인증 방식/JWT 상세 정책
   - 문서에는 Access/Refresh 토큰 구조만 제시되어 있고, 만료 시간/리프레시 정책/블랙리스트 전략은 미정.
   - 영향: `POST /auth/refresh`, `POST /auth/logout` 구현 방식 및 보안 수준.
   - 제안 옵션
     - Option A: 서버 측 Refresh 토큰 저장 + 로그아웃 시 무효화
     - Option B: 단순 클라이언트 보관 + 짧은 만료, 로그아웃은 클라이언트 삭제 위주
   - 결정 필요: 보안 요구 수준, 초기 런칭 스코프.

3. 프라이버시/공개 범위 정책 디테일
   - `FRIENDS` 기준, 회사명 마스킹 정책, 네트워크 공개 범위 등 상세 규칙이 일부 서술만 되어 있음.
   - 영향: `/me/privacy`, 네트워크/추천 API에서 데이터 필터링 로직.
   - 질문 예시
     - "FRIENDS"는 양방향 수락 기준인지, 단방향 팔로우인지?
     - 이직 이벤트는 어느 수준까지 모호화할지(회사명/산업/규모 등).

## 3. 오늘의 가정(Assumptions)
- 서버는 Spring Boot + Kotlin + JPA 기반으로 구현한다.
- 초기 DB는 로컬 H2 인메모리 또는 파일 기반으로 시작하고, 이후 Postgres로 마이그레이션 가능하도록 설계한다.
- JWT는 HS256 대칭키 기반으로 시작하며, 키는 환경 변수/설정 파일로 관리한다.

## 4. 내일/다음 사이클 작업 제안
- `build.gradle.kts` 확인 후 Spring Web/Security/Validation/JPA/H2 의존성 정리
- `application.yaml`에 local 프로파일 및 H2 설정 추가
- 헬스체크 엔드포인트 및 Auth 스켈레톤 컨트롤러 생성
- ERD를 참고해 User/UserProfile 엔티티 초안 정의 및 마이그레이션 전략 메모

## 5. 인수인계를 위한 메모
- 서버 관련 주요 문서 진입점
  - 기능/요구사항: `career_map_plan_doc.md`, `career_map_story_board.md`, `career_map_erd.md`
  - 서버 상태 요약: `server-status.md`
  - 서버 백로그: `server-next-steps.md`
  - 진행/이슈 로그: `threads/2025-12-19-careermap-server.md` (이 문서)
- 이후 개발자는 `server-next-steps.md`의 P0 항목부터 순서대로 작업하면 됨.

