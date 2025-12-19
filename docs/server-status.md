# CareerMap 서버 상태 요약 (2025-12-19)
- 서버 에이전트는 아래 `server-next-steps.md`에 정의된 백로그를 기준으로 작업을 진행하면 됨.
- 현재 레포는 CareerMap 서버의 초기 스켈레톤 상태로, ERD/플랜/스토리보드 문서에 정의된 API는 모두 새로 설계 및 구현해야 함.
## 6. 요약

- 로깅/모니터링: 아직 미설계/미구현
- 환경 구성: `src/main/resources/application.yaml` 존재 (세부 설정은 추후 보완 필요)
- 테스트: `src/test/kotlin/com/careermap/hamhub/HamhubApplicationTests.kt`만 존재, 도메인/컨트롤러 레벨 테스트는 아직 없음
## 5. 품질/운영 상태

**현재 코드에는 위 API들의 실제 구현이 존재하지 않음.**

   - 동료 이직 이벤트 기반 회사 예측/경로 추천
   - 동료 초대/요청/승인
5. 네트워크/예측 (문서 상 개념, API 스펙은 추후)

   - `GET /recommendations/{id}`
   - `GET /recommendations`
4. 추천

   - `GET /catalog/skills`
   - `GET /me/skills`, `PUT /me/skills`
   - `GET /me/projects`, `POST /me/projects`
   - `GET /me/experiences`, `POST /me/experiences`, `PUT /me/experiences/{id}`, `DELETE /me/experiences/{id}`
3. 이력: 경력/프로젝트/스킬

   - `PUT /me/privacy`
   - `PUT /me/profile`
   - `GET /me`
2. 온보딩/프로필

   - `POST /auth/logout`
   - `POST /auth/password/reset/request`
   - `POST /auth/refresh`
   - `POST /auth/login`
   - `POST /auth/signup`
1. 인증/회원가입

문서에 정의된 `/v1` API 목록:
### 4.2 API 구현 현황 (문서 vs 코드)

- 도메인/컨트롤러/서비스/리포지토리 계층은 아직 생성되지 않음
- `src/main/kotlin/com/careermap/hamhub/HamhubApplication.kt`만 존재하는 스켈레톤 상태
### 4.1 코드 구조
## 4. 구현 상태 요약

  - 프로필 공개 범위, 이직 이벤트 공개 범위, 회사명 마스킹 등
- 프라이버시/권한
  - 동료 추가/초대/승인, 네트워크 기반 회사 예측
- 네트워크(Friends/Network)
  - 추천 직장 목록/상세, 적합도 점수, 추천 이유, 갭/액션 아이템
- 추천(Recommendations)
  - 스킬: 이름/숙련도
  - 프로젝트: 이름/역할/기간/기여/임팩트 지표/스킬 태그
  - 경력 타임라인: 회사/팀/직책/기간/하이라이트/도메인 태그
- 이력(Experiences/Projects/Skills)
  - 현재 직무, 총 경력 개월 수, 위치, 근무 형태, 희망 직무/산업, 연봉 범위, 회사 규모 선호
- 프로필/Profile
  - 이메일 가입/로그인, 토큰(Access/Refresh), 비밀번호 재설정, 로그아웃
- 사용자/인증(Auth/User)

문서(`docs/career_map_erd.md`, `docs/career_map_plan_doc.md`, `docs/career_map_story_board.md`) 기준 핵심 도메인은 다음과 같음.
## 3. 도메인 개요 (문서 기준)

  - 로컬: `./gradlew bootRun`
- 실행 예상 명령:
- 프레임워크: Spring Boot (의존성은 `build.gradle.kts` 기준, 필요 시 갱신)
- 빌드: Gradle (Kotlin DSL)
- 언어: Kotlin
## 2. 기술 스택/런타임 (추정)

- 현재 시점: 도메인/설계 문서만 존재, 실제 비즈니스 API는 아직 미구현 상태
- 레포: `hamhub` (Kotlin + Spring Boot 스켈레톤)
- 서비스: CareerMap(커리어맵) 백엔드 API 서버
## 1. 서비스 및 레포 개요

