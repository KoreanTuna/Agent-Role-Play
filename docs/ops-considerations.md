# Operations Considerations (운영 고려사항)

BLOCKING: 배포/시크릿 관리 미정 — PM 호출 필요

요약: 서비스 운영을 위한 핵심 항목(스키마·마이그레이션·시드, 인증·권한, 레이트리밋·타임아웃, 로깅·모니터링, 백업·복구, 배포 파이프라인, 환경변수/시크릿 관리)과 권장값, 체크리스트를 정리합니다.

1) 스키마/마이그레이션/시드
- 권장 도구: Flyway 또는 Liquibase (Kotlin + Spring Boot 호환성 고려 시 Flyway 권장)
- 마이그레이션 전략:
  - 모든 DDL 변경은 버전 관리(migration scripts)로 적용
  - 마이그레이션은 CI 파이프라인에서 테스트(마이그레이션 적용 + 애플리케이션 시작)
- 시드 데이터:
  - 필수: 기본 카탈로그(스킬 목록, 회사 크기 카테고리), 관리자 계정(옵션)
  - 시드 방식: 환경별(dev/test) 스크립트 분리, 프로덕션은 최소화(관리자 수동 등록 권장)
- 체크리스트:
  - [ ] 개발 DB에서 마이그레이션 적용 테스트
  - [ ] 다운타임 정책 수립(online migration 권장)

2) 인증/권한
- 권장 아키텍처:
  - Access token: short-lived JWT (예: 15m~1h)
  - Refresh token: long-lived, 서버에서 안전하게 저장/무효화(예: Redis)
  - 로그인/회원가입: 이메일 기반 + (소셜 로그인은 선택)
- 권한 모델:
  - 기본 Role: ROLE_USER, ROLE_ADMIN
  - 리소스 기반 권한(예: 유저 자신의 리소스만 수정 가능)
- 토큰 관리:
  - Refresh 토큰은 DB 또는 Redis에 해시로 저장하고, 로그아웃 시 무효화
  - 토큰 재발급 로직 테스트(동시성/리플레이 공격 방어)
- 체크리스트:
  - [ ] 토큰 서명 키 관리(시크릿/키는 시크릿 매니저 사용)
  - [ ] 강한 비밀번호/비밀번호 해시(BCrypt 권장)

3) 레이트리밋/타임아웃/재시도
- 레이트리밋 권장값(예시):
  - 인증 관련(로그인/회원가입): 10 req/min/IP
  - 일반 API: 600 req/min/IP
  - 보호 대상(검색/추천): 120 req/min/IP
- 타임아웃:
  - API 게이트웨이(nginx/ALB): client timeout 60s, upstream 60s
  - 내부 서비스 호출: 2s~5s (권장)
- 재시도 정책:
  - 클라이언트는 idempotent한 요청(GET/PUT/DELETE)에 대해 지수 백오프 3회
  - 비정상 상태(5xx) 시 모니터 알람
- 체크리스트:
  - [ ] Rate limit 적용 위치(Nginx/API Gateway/서비스 내부)
  - [ ] Circuit Breaker(예: Resilience4j) 고려

4) 로깅/모니터링/알림
- 로깅:
  - 포맷: structured JSON logs (timestamp, level, service, traceId, userId, path, status)
  - 로그 레벨 정책: INFO(요약), DEBUG(개발), WARN/ERROR(이상)
  - 로그 수집: ELK/EFK(Stack) 또는 Datadog
- 모니터링:
  - 메트릭: request rate, error rate(4xx/5xx), latency percentiles(50/95/99), DB connections, queue depth
  - 툴: Prometheus + Grafana, 또는 SaaS(CloudWatch/DataDog)
- 알림:
  - PagerDuty/Slack 연동(중요 인시던트만)
- 체크리스트:
  - [ ] TraceId 연계(AWS X-Ray/Jaeger) 설정
  - [ ] SLO/SLA 정의 및 경고 임계값

5) 백업/복구
- DB 백업: 일일 논리 덤프 + 주간 전체 스냅샷
- 장애 시 복구 RTO/RPO 정의
- 체크리스트:
  - [ ] 자동 백업·복원 테스트
  - [ ] 백업 암호화 및 권한 관리

6) 배포 파이프라인
- 권장: CI(빌드/테스트) → CD(스테이징 자동배포) → 프로덕션 수동/자동(블루/그린 또는 롤링)
- 이미지/아티팩트 관리: Docker 이미지 태깅(semantic version) 후 레지스트리 업로드
- 마이그레이션 실행 시점: 유지보수 윈도우 또는 마이그레이션 먼저 → 앱 교체(순차)
- 체크리스트:
  - [ ] 배포 전 헬스체크(ready/liveness probe)
  - [ ] 롤백 시나리오 문서화

7) 환경 변수/비밀 관리
- 권장: 시크릿 매니저(예: AWS Secrets Manager, HashiCorp Vault) 사용
- 환경 변수 파일(.env)은 로컬/개발에 한정, 레포지토리 절대 커밋 금지
- 시크릿 접근 권한 최소화(서비스 계정별로 분리)
- BLOCKING 조건: 현재 레포지토리/문서에서 시크릿 저장/배포 방식이 명확하지 않음. PM 호출 필요.

8) 기타(성능/확장성)
- 캐싱: 추천/검색 결과는 TTL 기반 캐싱(redis) 권장
- 데이터 파이프라인: 추천 엔진 배치 작업은 별도 워커/스케줄러로 분리

부록: 빠른 체크리스트(요약)
- [ ] Flyway 설정 및 마이그레이션 스크립트
- [ ] JWT + Refresh 토큰 저장/무효화
- [ ] Rate limit 및 Circuit Breaker 적용
- [ ] Structured logging + Prometheus/Grafana
- [ ] Backup 정책 및 자동화
- [ ] Secrets Manager 도입 및 권한 정책

생성자: Plan 에이전트
생성일: 2025-12-19

