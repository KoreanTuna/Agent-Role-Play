# CareerMap 서버 Next Steps (백엔드 백로그)

## 0. 기준 문서
- ERD: `docs/career_map_erd.md`
- 기획/플랜: `docs/career_map_plan_doc.md`
- 스토리보드: `docs/career_map_story_board.md`
- PM 스레드: `docs/threads/2025-12-19-careermap-pm.md`

---

## 1. 1차(MVP) 서버 작업 백로그

### P0 (MVP 필수)
1. 프로젝트 러닝/기본 골격 정리
   - [ ] `build.gradle.kts` Spring Boot, Spring Web, Spring Security(JWT) 의존성 정리
   - [ ] `application.yaml`에 기본 프로파일(local) + H2 또는 Postgres 설정 추가
   - [ ] 헬스체크 엔드포인트 `GET /actuator/health` 또는 `GET /v1/health` 노출

2. Auth/회원가입/로그인
   - [ ] 도메인: `User`, `AuthToken` 엔티티/모델 정의
   - [ ] API: `POST /auth/signup`, `POST /auth/login`, `POST /auth/refresh`, `POST /auth/password/reset/request`, `POST /auth/logout`
   - [ ] JWT 기반 인증 필터/인터셉터 구성
   - [ ] 비밀번호 해시(BCrypt) 및 기본 유효성 검증(길이 등)

3. 프로필/온보딩
   - [ ] 도메인: `UserProfile`, `PrivacySettings`
   - [ ] API: `GET /me`, `PUT /me/profile`, `PUT /me/privacy`
   - [ ] 현재 유저 컨텍스트 추출(토큰 기반) 공통 컴포넌트

4. 이력(경력/프로젝트/스킬)
   - [ ] 도메인: `Experience`, `Project`, `Skill`, `UserSkill`
   - [ ] API: `GET/POST/PUT/DELETE /me/experiences`, `GET/POST /me/projects`, `GET/PUT /me/skills`, `GET /catalog/skills`
   - [ ] 커서 기반 페이지네이션 공통 응답 포맷 구현

5. 추천(직장 추천)
   - [ ] 도메인: `Recommendation`, `Company`, `JobRole`
   - [ ] API: `GET /recommendations`, `GET /recommendations/{id}`
   - [ ] 1차 룰 기반 추천 점수 계산(간단한 매칭 로직)
   - [ ] 추천 이유/갭/액션 구조 설계 및 더미 데이터 생성

6. 공통
   - [ ] 에러 응답 포맷 `{"error": { ... }}` 통일 구현
   - [ ] 글로벌 예외 핸들러(@ControllerAdvice)
   - [ ] 기본 로깅 설정 및 요청/응답 요약 로깅(민감정보 제외)

### P1 (MVP 이후 우선 개선)
1. 네트워크/동료
   - [ ] 동료 초대/요청/승인 도메인/테이블 초안 설계
   - [ ] 초대 코드/링크 기반 연결 API 설계 및 1차 구현

2. 예측/경로 추천
   - [ ] 네트워크 이직 이벤트 입력 API 초안
   - [ ] 단순 통계 기반 "Top N 회사" 예측 API 프로토타입

3. 어드민/운영
   - [ ] 기본 Admin 엔드포인트(신고 관리, 추천 품질 모니터링 등) 구조 설계

### P2 (실험/장기)
- [ ] 추천 피드백 루프(좋아요/숨기기/지원 여부) 기반 모델 개선 포인트 수집
- [ ] 네트워크 그래프 시각화용 데이터 API
- [ ] 고급 알림/리마인더 로직

---

## 2. 작업 방식
- 모든 설계 변경/결정 기록은 `docs/threads/YYYY-MM-DD-careermap-server.md`에 append
- 주요 아키텍처/도메인 결정은 `docs/decision-record-*.md` 형식으로 템플릿(`templates/decision-record.md`)을 사용해 남길 것
- 하루 작업 요약은 `templates/task-report.md`를 참고해 작성(예: `task-report-2025-12-19-server.md`)

---

## 3. 당일(2025-12-19) 서버 에이전트 우선 작업 제안
- [ ] 프로젝트 로컬 실행 가능 상태 만들기(의존성/설정 정리 + 헬스체크)
- [ ] Auth 도메인 설계 및 `/auth/signup`, `/auth/login` 스켈레톤 컨트롤러 구현
- [ ] 공통 에러 응답 포맷 및 글로벌 예외 핸들러 추가
- [ ] 진행/결정/질문은 `docs/threads/2025-12-19-careermap-server.md`에 기록
