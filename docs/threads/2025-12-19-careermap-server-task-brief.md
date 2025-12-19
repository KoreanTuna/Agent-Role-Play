# 업무 지시서 (Task Brief)

## 기본 정보
- 요청자: PM
- 담당 에이전트: 서버
- 요청 일자: 2025-12-19
- 목표 완료 일자: 2026-01-02 (중간 점검 2025-12-29)
- 우선순위: 높음

## 배경/목적
- 클라이언트 답변: 서버 레포 https://github.com/KoreanTuna/CareerMap-Api, Prod 1개 가정, 브랜칭 자율, 로컬 배포/테스트 우선. 모바일(Android/iOS) 전용, 모든 비즈니스 로직 테스트 필요.
- 2주 내 로컬 실행 가능 상태와 핵심 API 스켈레톤을 마련해 FE 병행 개발을 unblock.

## 요구 사항
- 레포 클론/셋업, 로컬 프로파일(H2 또는 Postgres) 구성, 헬스체크 노출.
- Auth: signup/login/refresh/logout/password reset request 스켈레톤.
- 프로필/프라이버시: GET/PUT /me, /me/privacy.
- 이력: 경력/프로젝트/스킬 CRUD, catalog/skills, 커서 페이지네이션 공통 응답.
- 추천: 리스트/상세 스켈레톤(룰 기반 더미).
- 공통 에러 응답 포맷 및 글로벌 예외 핸들러.
- 비즈니스 로직 단위/통합 테스트(서비스/도메인/핵심 핸들러).

## 범위 (포함/제외)
- 포함: 로컬 실행/헬스체크, 핵심 도메인 스켈레톤(API/서비스/도메인), 공통 에러 포맷, 테스트.
- 제외: Prod 배포/CI, 소셜 로그인, 네트워크/예측 고도화, 다중 Stage 환경.

## 산출물
- 서버 PR, README/로컬 실행 가이드, 테스트 결과.
- 진행/차단/다음 액션을 스레드에 업데이트.

## 참고 자료
- docs/career_map_erd.md
- docs/server-next-steps.md
- docs/plans/sprint1-2025-12-19.md
- docs/threads/2025-12-19-careermap-pm.md

## 체크리스트
- [ ] 이해 완료
- [ ] 일정 확인
- [ ] 리스크 식별
