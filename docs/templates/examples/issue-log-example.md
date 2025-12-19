# 이슈/리스크 기록 (Issue Log) - 작성 예시

> 이 문서는 issue-log.md 템플릿의 작성 예시입니다.

## 기본 정보
- 작성자: 프론트엔드 에이전트
- 발생 일자: 2025-12-22
- 관련 업무: 로그인 화면 구현 (docs/threads/2025-12-19-login-screen.md)
- 심각도: 높음

## 이슈/리스크 설명

### 문제 상황
로그인 API 연동 테스트 중 CORS(Cross-Origin Resource Sharing) 에러 발생.
Flutter Web에서 로컬 개발 시 `http://localhost:5000`으로 API 호출 시 브라우저가 요청을 차단합니다.

### 에러 메시지
```
Access to XMLHttpRequest at 'http://localhost:5000/v1/auth/login'
from origin 'http://localhost:8080' has been blocked by CORS policy:
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

### 재현 단계
1. Flutter Web 개발 서버 실행: `flutter run -d chrome`
2. 로그인 화면에서 이메일/비밀번호 입력
3. 로그인 버튼 클릭
4. API 호출 실패 (네트워크 탭에서 CORS 에러 확인)

### 발생 환경
- Flutter: 3.16.0
- 브라우저: Chrome 120.0.6099.109
- 서버: Kotlin Spring Boot (로컬 실행)
- OS: macOS 14.2

## 영향 범위

### 차단 여부
- **차단됨**: 로그인 기능 테스트 불가
- Flutter Web 개발 환경에서만 발생 (Android/iOS는 정상)

### 영향 받는 기능
- 로그인 API 연동 테스트
- 추후 모든 API 호출 (회원가입, 이력 조회 등)

### 영향 받는 팀
- 프론트엔드: 개발 진행 차단
- 서버: CORS 설정 필요
- QA: Web 플랫폼 테스트 불가

### 일정 영향
- 로그인 화면 완료 목표: 12/26 → **12/27로 1일 지연 예상**

## 대응/완화 방안

### 즉시 조치 (프론트엔드)
1. **임시 우회**: Android/iOS 에뮬레이터에서 개발 진행
   - 장점: CORS 영향 없음, 개발 계속 가능
   - 단점: Web 플랫폼 테스트 불가

2. **프록시 설정**: Flutter 개발 서버에 프록시 추가
   ```bash
   # chrome --disable-web-security 플래그 (비권장)
   # 또는 CORS proxy 도구 사용
   ```

### 근본 해결 (서버팀 요청)
**@서버 에이전트** - 다음 설정을 Spring Boot에 추가해주세요:

```kotlin
@Configuration
class WebConfig : WebMvcConfigurer {
    override fun addCorsMappings(registry: CorsRegistry) {
        registry.addMapping("/**")
            .allowedOrigins("http://localhost:8080") // Flutter Web 개발 서버
            .allowedMethods("GET", "POST", "PUT", "DELETE")
            .allowedHeaders("*")
            .allowCredentials(true)
    }
}
```

또는 개발 환경 전용 필터:

```kotlin
@Component
@Profile("dev")
class CorsFilter : Filter {
    override fun doFilter(request: ServletRequest, response: ServletResponse, chain: FilterChain) {
        val res = response as HttpServletResponse
        res.setHeader("Access-Control-Allow-Origin", "http://localhost:8080")
        res.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE")
        res.setHeader("Access-Control-Allow-Headers", "*")
        chain.doFilter(request, response)
    }
}
```

### 검증 계획
1. 서버팀이 CORS 설정 적용
2. 서버 재시작 후 Flutter Web에서 로그인 재테스트
3. 정상 동작 확인 → 이슈 종료

## 결정 사항

### 협의 필요
- [ ] 서버팀: CORS 설정 추가 가능 여부 및 일정
- [ ] PM: 일정 조정 (12/27로 1일 연장) 승인

### 임시 결정 (프론트엔드)
- ✅ 당장은 Android 에뮬레이터에서 개발 진행
- ✅ Web 테스트는 서버 CORS 설정 완료 후 진행

## 후속 작업

### 서버팀
1. CORS 설정 추가 (우선순위: 높음)
   - 예상 소요: 30분
   - 완료 목표: 2025-12-22 EOD

2. 환경별 CORS 정책 문서화
   - 개발: `localhost:*` 허용
   - 스테이징: 스테이징 도메인만
   - 프로덕션: 프로덕션 도메인만

### 프론트엔드
1. 서버 CORS 설정 완료 후 Web 테스트
2. 환경별 API 베이스 URL 설정 문서화
   - 개발: `http://localhost:5000`
   - 스테이징: TBD
   - 프로덕션: TBD

### 문서화
1. 로컬 개발 환경 설정 가이드에 CORS 이슈 추가
2. 트러블슈팅 섹션 작성

---

**작성자**: 프론트엔드 에이전트
**업데이트**: 2025-12-22
**상태**: 해결 대기 중 (서버팀 조치 필요)
