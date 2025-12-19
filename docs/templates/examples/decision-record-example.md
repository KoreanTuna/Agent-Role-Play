# 의사결정 기록 (Decision Record) - 작성 예시

> 이 문서는 decision-record.md 템플릿의 작성 예시입니다.

## 기본 정보
- 결정 주체: PM 에이전트 (서버, 프론트엔드 의견 반영)
- 결정 일자: 2025-12-20
- 관련 이슈/업무: 인증 토큰 저장 방식 결정

## 문제/배경

### 상황
CareerMap 앱에서 로그인 성공 시 받은 JWT 토큰(accessToken, refreshToken)을 어디에 저장할지 결정이 필요합니다.

### 왜 중요한가
- 잘못된 저장 방식 선택 시 **보안 취약점** 발생
- 사용자 경험에 영향 (자동 로그인, 로그아웃 동작)
- 플랫폼별 제약 고려 필요 (Android, iOS, Web)

### 제약 조건
- Flutter 앱: Android, iOS, Web 모두 지원
- refreshToken은 민감 정보로 안전한 저장 필수
- Web 환경에서는 보안 제약이 더 엄격함

## 선택지

### 1. SharedPreferences (현재 가장 일반적)
**장점**:
- Flutter 기본 제공 (`shared_preferences` 패키지)
- 모든 플랫폼(Android/iOS/Web) 지원
- 간단한 API, 빠른 구현

**단점**:
- Android: 일반 파일로 저장 (루팅된 기기에서 접근 가능)
- iOS: UserDefaults 사용 (암호화 X)
- Web: LocalStorage 사용 (**XSS 공격에 취약**)
- refreshToken 같은 민감 정보 저장 부적합

**예상 코드**:
```dart
final prefs = await SharedPreferences.getInstance();
await prefs.setString('accessToken', token);
```

---

### 2. flutter_secure_storage (추천)
**장점**:
- Android: KeyStore 활용 (하드웨어 암호화)
- iOS: Keychain 활용 (OS 레벨 암호화)
- Web: 제한적 지원 (WebCrypto API)
- refreshToken 저장에 적합

**단점**:
- Web 지원 제한적 (일부 브라우저에서 동작 불안정)
- 초기 설정 필요 (Android minSdkVersion 18+)

**예상 코드**:
```dart
final storage = FlutterSecureStorage();
await storage.write(key: 'accessToken', value: token);
```

---

### 3. 하이브리드 접근 (secure_storage + in-memory)
**장점**:
- refreshToken: secure_storage에 저장 (장기 보관)
- accessToken: 메모리에만 보관 (앱 실행 중)
- Web 환경에서도 보안성 높음 (메모리는 XSS로 접근 어려움)

**단점**:
- 구현 복잡도 증가
- 앱 재시작 시 accessToken 재발급 필요 (refreshToken 사용)

**예상 코드**:
```dart
// refreshToken만 저장
await secureStorage.write(key: 'refreshToken', value: refreshToken);

// accessToken은 메모리 (Riverpod state)
ref.read(authProvider.notifier).setAccessToken(accessToken);
```

## 결정 내용

**선택**: **2번 - flutter_secure_storage 사용**

### 저장 방식
- **accessToken**: `flutter_secure_storage`에 저장
- **refreshToken**: `flutter_secure_storage`에 저장
- Web 환경: 당장은 secure_storage 사용, 추후 브라우저 호환성 이슈 발생 시 하이브리드 방식 재검토

### 플랫폼별 동작
- Android: KeyStore 활용
- iOS: Keychain 활용
- Web: WebCrypto API (제한적이나 MVP는 가능)

## 결정 근거

### 보안성 우선
- refreshToken은 30일 유효 → 탈취 시 장기간 악용 가능
- SharedPreferences는 Web에서 XSS 공격에 취약
- **보안 > 편의성** 원칙

### 플랫폼 지원
- Android/iOS는 secure_storage가 업계 표준
- Web은 제한적이나 MVP 범위에서는 동작 확인됨
- 추후 Web 전용 보안 강화 필요 시 하이브리드 방식 적용 가능

### 구현 복잡도
- 하이브리드 방식(3번)은 현 시점에 과도한 엔지니어링
- MVP는 2번으로 충분, 추후 개선 가능

### 레퍼런스
- 주요 Flutter 앱들(예: GitHub 모바일)이 secure_storage 사용
- OWASP Mobile Top 10에서 권장하는 방식

## 영향/후속 작업

### 코드 변경
- **프론트엔드**:
  1. `flutter_secure_storage` 패키지 추가 (`pubspec.yaml`)
  2. `lib/core/storage/token_storage.dart` 생성
     - `saveTokens()`, `getAccessToken()`, `getRefreshToken()`, `deleteTokens()`
  3. 로그인 성공 시 토큰 저장 로직 추가
  4. API 호출 시 헤더에 토큰 포함 (`Authorization: Bearer <token>`)
  5. 401 응답 시 refreshToken으로 재발급 로직

- **서버**:
  - 변경 없음 (이미 JWT 발급 중)

### 문서 업데이트
- **문서화 에이전트**:
  1. 로컬 개발 환경 가이드에 Android minSdkVersion 18+ 명시
  2. 보안 정책 문서에 토큰 저장 방식 기록
  3. 트러블슈팅: Web 환경에서 secure_storage 이슈 발생 시 대응 방법

### 테스트 계획
- **프론트엔드**:
  1. Android/iOS 실기기에서 토큰 저장/조회 테스트
  2. 앱 종료 후 재시작 시 토큰 유지 확인
  3. 로그아웃 시 토큰 삭제 확인
  4. Web 브라우저 3종(Chrome/Safari/Firefox)에서 동작 확인

### 일정 영향
- 구현 추가 소요: 0.5일 (원래 계획에 포함됨)
- 테스트 추가 소요: 0.5일
- **전체 일정 변경 없음**

### 향후 재검토 시점
1. **Web 브라우저 호환성 이슈 발생 시**
   - 하이브리드 방식(3번)으로 전환 검토

2. **프로덕션 배포 전 보안 감사 시**
   - 외부 보안 전문가 의견 반영

3. **사용자 100명 달성 시**
   - 실사용 데이터 기반 보안 강화 검토

## 관련 링크
- Flutter Secure Storage 공식 문서: https://pub.dev/packages/flutter_secure_storage
- OWASP Mobile Security: https://owasp.org/www-project-mobile-top-10/
- 관련 스레드: docs/threads/2025-12-19-login-screen.md

---

**작성자**: PM 에이전트
**승인자**: PM, 프론트엔드, 서버 에이전트 (합의)
**상태**: 확정 (Approved)
