# Server Integration & API Mapping

요약: 문서(ERD/플랜/스토리보드)와 코드베이스를 대조한 서버-API 매핑 결과입니다.

BLOCKING: 배포/시크릿 관리 미정 — PM 호출 필요

1) 서브모듈 업데이트
- `docs` 서브모듈을 원격에서 업데이트했습니다. 변경 내역이 로컬에 반영되어 문서 스캔을 진행했습니다.

2) ERD/도메인(문서 기반 요약)
- 추출된 엔티티 목록(문서 요약):
  - User
    - id (uuid)
    - email
    - passwordHash
    - nickname
    - createdAt
    - privacy settings (profileVisibility, moveVisibility, maskCompanyNameInMoves)
    - roles (USER, ADMIN)
  - Profile (온보딩/요약)
    - currentRole
    - totalMonths
    - location
    - workStyle
    - desiredRoles
    - industries
    - salaryRange (min, max)
    - companySize
    - resumeCompletion
  - Experience
    - id (uuid)
    - userId (fk)
    - companyName
    - teamName
    - title
    - employmentType
    - startDate
    - endDate
    - isCurrent
    - highlights
    - domainTags
  - Project
    - id
    - userId
    - name
    - role
    - startDate
    - endDate
    - contribution
    - impactMetrics
    - skillTags
  - Skill (catalog)
    - id
    - name
    - level
  - Recommendation
    - id
    - userId
    - company (id, name)
    - roleTitle
    - fitScore
    - reasons
    - gaps
  - Network / Colleague / MoveEvent
    - colleague relations (user-user)
    - move events (company, date, visibility)
  - Auth tokens
    - accessToken (JWT)
    - refreshToken

3) 코드베이스에서 발견된 엔드포인트(요약)
- 스캔 결과: `src/main/kotlin` 내부에 REST 컨트롤러 어노테이션(@RestController, @Controller, @RequestMapping, @GetMapping 등)을 발견하지 못했습니다. 현재 서버 코드에는 엔드포인트 구현이 없는 것으로 판단됩니다.

엔드포인트 매핑(문서 기반 권장 표)
| 엔드포인트 | 메서드 | 경로 | 설명 | 요청 요약 | 응답 요약 | 인증 |
|---|---:|---|---|---|---|---|
| Signup | POST | /v1/auth/signup | 이메일 회원가입 | email, password, nickname, agreements | user, tokens | 없음 |
| Login | POST | /v1/auth/login | 이메일 로그인 | email, password | user, tokens | 없음 |
| Refresh | POST | /v1/auth/refresh | 토큰 갱신 | refreshToken | accessToken, refreshToken | 없음 |
| Logout | POST | /v1/auth/logout | 로그아웃(리프레시 토큰 폐기) | refreshToken | {ok:true} | 인증(토큰 필요) |
| Get profile | GET | /v1/me | 내 기본 정보 조회 | - | user + profile + privacy | 인증 필요 |
| Update profile | PUT | /v1/me/profile | 온보딩/프로필 수정 | profile fields | {ok:true} | 인증 필요 |
| Experiences list | GET | /v1/me/experiences | 경력 리스트(커서) | cursor | items, nextCursor | 인증 필요 |
| Create experience | POST | /v1/me/experiences | 경력 추가 | experience body | {id} | 인증 필요 |
| ... | | | 문서에 상세 명세로 다수 존재 | | | |

(참고: 위 표의 엔드포인트는 `docs/docs/career_map_erd.md` 문서의 API 명세를 그대로 요약한 권장 명세입니다. 코드에 구현이 없는 경우 아래 '누락 엔드포인트'에 기록했습니다.)

4) 문서-코드 간 갭(누락 엔드포인트)
- 현재 코드베이스에서 서버 엔드포인트가 발견되지 않아 문서상의 API 대부분이 미구현 상태로 보입니다. 주요 누락 항목 예시(우선순위 제안):
  - [High] 인증/회원가입/로그인/토큰 관련 엔드포인트 (경로: /v1/auth/*)
    - 제안: POST /v1/auth/signup, POST /v1/auth/login, POST /v1/auth/refresh, POST /v1/auth/logout
  - [High] 사용자 프로필/온보딩 엔드포인트 (경로: /v1/me, /v1/me/profile, /v1/me/privacy)
  - [High] 이력(Experiences/Projects/Skills/Catalog) CRUD
  - [High] 추천 API (GET /v1/recommendations, GET /v1/recommendations/{id})
  - [Medium] 네트워크/동료 초대/친구 요청 관련 엔드포인트
  - [Medium] 어드민(유저/신고 관리, 사전 관리) 엔드포인트

누락 엔드포인트 샘플 스펙(예: 회원가입)
- 경로: POST /v1/auth/signup
- 요청 예시:
{
  "email": "name@example.com",
  "password": "password1234",
  "nickname": "minwoo",
  "agreements": {"terms": true, "privacy": true, "marketing": false}
}
- 응답 예시: 201
{
  "user": {"id": "uuid","email": "name@example.com","nickname": "minwoo","createdAt": "..."},
  "tokens": {"accessToken":"jwt","refreshToken":"jwt","expiresInSec":3600}
}

5) 권장 작업
- 우선순위 High 항목을 먼저 구현하세요: Auth, Profile, Experiences, Recommendations.
- API 계약(openapi/Swagger) 먼저 정의 후(문서화), 컨트롤러·서비스·레포지토리 순으로 구현 권장.
- 테스트: 단위 테스트 + 통합 테스트(인메모리 DB or testcontainers)를 작성.

6) 추가 메모
- 코드에서 배포/환경변수/비밀관리 관련 명확한 규칙을 찾지 못했습니다. 이 항목은 명확화 필요(BLOCKING). PM 호출을 권고합니다.

---

생성자: 자동 스캔 스크립트(Plan 에이전트)
생성일: 2025-12-19

