# CareerMap 디자인 시스템 가이드

## 개요

CareerMap의 디자인 시스템은 Material Design 3를 기반으로 하며, 커리어 네비게이션 앱의 특성에 맞게 전문성, 신뢰감, 미래 지향성을 표현합니다.

### 디자인 원칙

1. **명확성 (Clarity)**: 복잡한 커리어 데이터를 명확하고 이해하기 쉽게 전달
2. **전문성 (Professionalism)**: 비즈니스 환경에 어울리는 세련되고 신뢰감 있는 디자인
3. **일관성 (Consistency)**: 모든 화면과 컴포넌트에서 일관된 경험 제공
4. **접근성 (Accessibility)**: 모든 사용자가 쉽게 사용할 수 있는 디자인

---

## 1. 컬러 시스템

### 1.1 Primary Colors (주요 색상)

**Primary - Indigo Blue**
- 목적: 브랜드 정체성, 주요 액션, 중요한 UI 요소
- 의미: 전문성, 신뢰감, 안정성

```
Primary 100: #E8EAF6  (매우 밝음 - 배경, 비활성)
Primary 200: #C5CAE9  (밝음 - hover, 보조)
Primary 300: #9FA8DA  (중간 밝음)
Primary 400: #7986CB  (중간)
Primary 500: #5C6BC0  (기본) ★ Main
Primary 600: #3949AB  (중간 어두움 - pressed)
Primary 700: #303F9F  (어두움)
Primary 800: #283593  (매우 어두움)
Primary 900: #1A237E  (극도로 어두움)
```

### 1.2 Secondary Colors (보조 색상)

**Secondary - Teal Green**
- 목적: 강조, 성공, 성장 표현
- 의미: 발전, 가능성, 긍정적 변화

```
Secondary 100: #E0F2F1
Secondary 200: #B2DFDB
Secondary 300: #80CBC4
Secondary 400: #4DB6AC
Secondary 500: #26A69A  ★ Main
Secondary 600: #00897B  (pressed)
Secondary 700: #00796B
Secondary 800: #00695C
Secondary 900: #004D40
```

### 1.3 Accent Colors (강조 색상)

**Accent - Amber**
- 목적: 중요한 정보 강조, 알림, 새로운 콘텐츠
- 의미: 주목, 기회, 주의

```
Accent 100: #FFF8E1
Accent 200: #FFECB3
Accent 300: #FFE082
Accent 400: #FFD54F
Accent 500: #FFCA28  ★ Main
Accent 600: #FFB300
Accent 700: #FFA000
Accent 800: #FF8F00
Accent 900: #FF6F00
```

### 1.4 Neutral Colors (중립 색상)

**Gray Scale**
- 목적: 텍스트, 배경, 구분선, 비활성 상태

```
Gray 50:  #FAFAFA  (배경 - 최상위)
Gray 100: #F5F5F5  (배경 - 카드)
Gray 200: #EEEEEE  (배경 - 구분)
Gray 300: #E0E0E0  (구분선 - 밝음)
Gray 400: #BDBDBD  (구분선 - 기본)
Gray 500: #9E9E9E  (비활성 텍스트)
Gray 600: #757575  (보조 텍스트)
Gray 700: #616161  (일반 텍스트)
Gray 800: #424242  (중요 텍스트)
Gray 900: #212121  (제목, 헤더)
```

### 1.5 Semantic Colors (의미 색상)

**Success - Green**
```
Success Light:  #C8E6C9
Success Main:   #4CAF50
Success Dark:   #388E3C
```

**Warning - Orange**
```
Warning Light:  #FFE0B2
Warning Main:   #FF9800
Warning Dark:   #F57C00
```

**Error - Red**
```
Error Light:  #FFCDD2
Error Main:   #F44336
Error Dark:   #D32F2F
```

**Info - Blue**
```
Info Light:  #BBDEFB
Info Main:   #2196F3
Info Dark:   #1976D2
```

### 1.6 Background & Surface

```
Background:        #FAFAFA  (Gray 50)
Surface:           #FFFFFF  (White)
Surface Variant:   #F5F5F5  (Gray 100)
```

### 1.7 Text Colors

```
Text Primary:      #212121  (Gray 900) - 제목, 중요 텍스트
Text Secondary:    #757575  (Gray 600) - 보조 텍스트, 설명
Text Disabled:     #BDBDBD  (Gray 400) - 비활성 텍스트
Text Hint:         #9E9E9E  (Gray 500) - 플레이스홀더, 힌트

On Primary:        #FFFFFF  (Primary 버튼 위 텍스트)
On Secondary:      #FFFFFF  (Secondary 버튼 위 텍스트)
On Surface:        #212121  (Surface 위 텍스트)
On Error:          #FFFFFF  (Error 배경 위 텍스트)
```

### 1.8 Overlay & Elevation

```
Overlay Light:     #000000 with 4% opacity   (hover)
Overlay Medium:    #000000 with 8% opacity   (pressed)
Overlay Dark:      #000000 with 12% opacity  (drag)

Scrim:             #000000 with 32% opacity  (모달 배경)
```

---

## 2. 타이포그래피

### 2.1 폰트 패밀리

**Primary Font: Pretendard**
- 한글과 영문 모두 지원
- 가독성이 뛰어나고 현대적인 느낌
- 다양한 폰트 굵기 지원 (Thin ~ Black)
- 오픈소스 무료 라이선스

```dart
// Flutter implementation
fontFamily: 'Pretendard'
```

**Fallback**
```
iOS: SF Pro, -apple-system
Android: Roboto
```

### 2.2 타입 스케일 (Type Scale)

#### Display (디스플레이)
- 용도: 대형 제목, 랜딩 페이지, 주요 메시지
- 빈도: 드물게 사용

```
Display Large:
  fontSize: 57px / 3.56rem
  lineHeight: 64px / 1.12
  fontWeight: 400 (Regular)
  letterSpacing: -0.25px

Display Medium:
  fontSize: 45px / 2.81rem
  lineHeight: 52px / 1.16
  fontWeight: 400 (Regular)
  letterSpacing: 0px

Display Small:
  fontSize: 36px / 2.25rem
  lineHeight: 44px / 1.22
  fontWeight: 400 (Regular)
  letterSpacing: 0px
```

#### Headline (헤드라인)
- 용도: 화면 제목, 섹션 제목
- 빈도: 화면당 1-2회

```
Headline Large:
  fontSize: 32px / 2rem
  lineHeight: 40px / 1.25
  fontWeight: 600 (SemiBold)
  letterSpacing: 0px

Headline Medium:
  fontSize: 28px / 1.75rem
  lineHeight: 36px / 1.29
  fontWeight: 600 (SemiBold)
  letterSpacing: 0px

Headline Small:
  fontSize: 24px / 1.5rem
  lineHeight: 32px / 1.33
  fontWeight: 600 (SemiBold)
  letterSpacing: 0px
```

#### Title (타이틀)
- 용도: 카드 제목, 리스트 아이템 제목
- 빈도: 자주 사용

```
Title Large:
  fontSize: 22px / 1.375rem
  lineHeight: 28px / 1.27
  fontWeight: 600 (SemiBold)
  letterSpacing: 0px

Title Medium:
  fontSize: 16px / 1rem
  lineHeight: 24px / 1.5
  fontWeight: 600 (SemiBold)
  letterSpacing: 0.15px

Title Small:
  fontSize: 14px / 0.875rem
  lineHeight: 20px / 1.43
  fontWeight: 600 (SemiBold)
  letterSpacing: 0.1px
```

#### Body (본문)
- 용도: 일반 텍스트, 설명, 내용
- 빈도: 가장 많이 사용

```
Body Large:
  fontSize: 16px / 1rem
  lineHeight: 24px / 1.5
  fontWeight: 400 (Regular)
  letterSpacing: 0.5px

Body Medium:
  fontSize: 14px / 0.875rem
  lineHeight: 20px / 1.43
  fontWeight: 400 (Regular)
  letterSpacing: 0.25px

Body Small:
  fontSize: 12px / 0.75rem
  lineHeight: 16px / 1.33
  fontWeight: 400 (Regular)
  letterSpacing: 0.4px
```

#### Label (레이블)
- 용도: 버튼 텍스트, 탭, 칩
- 빈도: 자주 사용

```
Label Large:
  fontSize: 14px / 0.875rem
  lineHeight: 20px / 1.43
  fontWeight: 500 (Medium)
  letterSpacing: 0.1px

Label Medium:
  fontSize: 12px / 0.75rem
  lineHeight: 16px / 1.33
  fontWeight: 500 (Medium)
  letterSpacing: 0.5px

Label Small:
  fontSize: 11px / 0.688rem
  lineHeight: 16px / 1.45
  fontWeight: 500 (Medium)
  letterSpacing: 0.5px
```

### 2.3 폰트 굵기 (Font Weight)

```
Thin:       100 (사용 제한)
Light:      300 (보조 정보)
Regular:    400 (기본 텍스트)
Medium:     500 (레이블, 강조)
SemiBold:   600 (제목, 헤더)
Bold:       700 (강한 강조)
ExtraBold:  800 (사용 제한)
Black:      900 (사용 제한)
```

**권장 사용:**
- 일반 텍스트: Regular (400)
- 버튼/탭/칩: Medium (500)
- 제목/헤더: SemiBold (600)
- 특별 강조: Bold (700)

---

## 3. 간격 시스템 (Spacing)

### 3.1 기본 그리드

**8pt Grid System**
- 모든 간격은 4의 배수 사용
- 기본 단위: 4px (0.25rem)

```
xs:   4px   (0.25rem)  - 아주 작은 간격 (아이콘-텍스트)
sm:   8px   (0.5rem)   - 작은 간격 (칩 내부 padding)
md:   12px  (0.75rem)  - 중간 작은 간격
base: 16px  (1rem)     - 기본 간격 ★ 가장 많이 사용
lg:   20px  (1.25rem)  - 중간 큰 간격
xl:   24px  (1.5rem)   - 큰 간격 (카드 padding)
2xl:  32px  (2rem)     - 매우 큰 간격 (섹션 간격)
3xl:  40px  (2.5rem)   - 초대형 간격
4xl:  48px  (3rem)     - 초대형 간격
5xl:  64px  (4rem)     - 극대형 간격 (페이지 상하 여백)
```

### 3.2 컴포넌트별 기본 간격

#### Card
```
Padding:        24px (xl)
Gap (내부):     16px (base)
Margin (하단):  16px (base)
```

#### List Item
```
Padding (수평): 16px (base)
Padding (수직): 12px (md)
Gap:            12px (md)
```

#### Button
```
Padding (Large):    16px 32px
Padding (Medium):   12px 24px
Padding (Small):    8px 16px
Gap (아이콘):       8px (sm)
```

#### Input Field
```
Padding (내부):     16px (base)
Gap (레이블):       8px (sm)
Margin (하단):      16px (base)
```

#### Bottom Sheet / Modal
```
Padding (수평):     24px (xl)
Padding (상단):     24px (xl)
Padding (하단):     32px (2xl) + Safe Area
Gap (섹션):         24px (xl)
```

#### Screen Container
```
Padding (수평):     16px (base)
Padding (상단):     16px (base)
Padding (하단):     16px (base) + Safe Area
Gap (섹션):         24px (xl)
```

---

## 4. Elevation & Shadow (고도 및 그림자)

Material Design 3의 elevation 시스템 사용

### 4.1 Elevation Levels

```
Level 0: 0dp
  shadow: none
  용도: 배경, 평면 요소

Level 1: 1dp
  shadow: 0px 1px 2px rgba(0, 0, 0, 0.06)
  용도: Card (기본), Chip

Level 2: 3dp
  shadow: 0px 2px 4px rgba(0, 0, 0, 0.08)
  용도: Card (hover), FAB (rest)

Level 3: 6dp
  shadow: 0px 4px 8px rgba(0, 0, 0, 0.12)
  용도: Card (pressed), App Bar, Bottom Nav

Level 4: 8dp
  shadow: 0px 6px 12px rgba(0, 0, 0, 0.16)
  용도: Bottom Sheet, Modal

Level 5: 12dp
  shadow: 0px 8px 16px rgba(0, 0, 0, 0.20)
  용도: Dialog, Popup Menu
```

### 4.2 컴포넌트별 Elevation

```
App Bar:              Level 3 (6dp)
Bottom Navigation:    Level 3 (6dp)
Card (rest):          Level 1 (1dp)
Card (hover):         Level 2 (3dp)
FAB (rest):           Level 2 (3dp)
FAB (hover):          Level 3 (6dp)
Dialog:               Level 5 (12dp)
Bottom Sheet:         Level 4 (8dp)
Snackbar:             Level 3 (6dp)
```

---

## 5. Border Radius (모서리 반경)

### 5.1 Radius Scale

```
none:   0px     - 사각형 (구분선, 일부 Input)
xs:     2px     - 매우 작음
sm:     4px     - 작음 (Chip, Tag)
md:     8px     - 중간 (Button, Input) ★ 기본
lg:     12px    - 중간 큰 (Card)
xl:     16px    - 큰 (Modal, Bottom Sheet)
2xl:    20px    - 매우 큰
3xl:    24px    - 초대형
full:   9999px  - 완전 둥근 (Avatar, Badge)
```

### 5.2 컴포넌트별 Border Radius

```
Button:             8px  (md)
Input Field:        8px  (md)
Card:               12px (lg)
Bottom Sheet:       16px (xl) - 상단만
Modal/Dialog:       16px (xl)
Chip/Tag:           4px  (sm)
Avatar:             9999px (full)
Badge:              9999px (full)
Image Thumbnail:    8px  (md)
FAB:                16px (xl)
```

---

## 6. 아이콘 시스템

### 6.1 아이콘 라이브러리

**Material Symbols (권장)**
- Material Design 3 아이콘
- Outlined / Rounded / Sharp 스타일
- CareerMap: **Rounded** 스타일 사용 (부드럽고 현대적)

**Fallback: Material Icons**

### 6.2 아이콘 크기

```
xs:     16px  - 작은 아이콘 (칩, 태그 내부)
sm:     20px  - 작은 아이콘 (리스트 아이템)
md:     24px  - 기본 아이콘 ★ 가장 많이 사용
lg:     32px  - 큰 아이콘 (FAB, 강조)
xl:     48px  - 매우 큰 아이콘 (일러스트, 빈 상태)
2xl:    64px  - 초대형 아이콘 (온보딩, 빈 상태)
```

### 6.3 하단 탭 아이콘

```
홈:        home_rounded
이력:      work_rounded / badge_rounded
추천:      recommend_rounded / stars_rounded
네트워크:  groups_rounded / people_rounded
예측:      insights_rounded / analytics_rounded
```

### 6.4 주요 액션 아이콘

```
추가:      add_rounded
편집:      edit_rounded
삭제:      delete_rounded
검색:      search_rounded
필터:      filter_list_rounded
정렬:      sort_rounded
설정:      settings_rounded
알림:      notifications_rounded
더보기:    more_vert_rounded
닫기:      close_rounded
뒤로가기:  arrow_back_rounded
다음:      arrow_forward_rounded
체크:      check_rounded
정보:      info_rounded
경고:      warning_rounded
에러:      error_rounded
```

---

## 7. 컴포넌트 가이드

### 7.1 Button

#### Primary Button (주요 액션)
```
Background:     Primary 500 (#5C6BC0)
Text Color:     White (#FFFFFF)
Font:           Label Large (14px, Medium 500)
Padding:        16px 32px (Large) / 12px 24px (Medium)
Border Radius:  8px
Elevation:      Level 0 (rest), Level 1 (hover)
Min Width:      64px
Min Height:     40px (Medium), 48px (Large)

States:
- Hover:        Primary 600 배경
- Pressed:      Primary 600 배경 + Overlay Medium
- Disabled:     Gray 300 배경, Gray 500 텍스트
```

#### Secondary Button (보조 액션)
```
Background:     Transparent
Border:         1px solid Primary 500
Text Color:     Primary 500
Font:           Label Large (14px, Medium 500)
Padding:        16px 32px (Large) / 12px 24px (Medium)
Border Radius:  8px
Min Height:     40px (Medium), 48px (Large)

States:
- Hover:        Primary 100 배경
- Pressed:      Primary 200 배경
- Disabled:     Gray 300 테두리, Gray 500 텍스트
```

#### Text Button (약한 액션)
```
Background:     Transparent
Text Color:     Primary 500
Font:           Label Large (14px, Medium 500)
Padding:        8px 16px
Border Radius:  8px

States:
- Hover:        Overlay Light
- Pressed:      Overlay Medium
- Disabled:     Gray 500 텍스트
```

### 7.2 Input Field / TextField

```
Background:     Surface (#FFFFFF)
Border:         1px solid Gray 400
Text Color:     Gray 900
Placeholder:    Gray 500
Font:           Body Large (16px, Regular 400)
Padding:        16px
Border Radius:  8px
Height:         56px (Single Line)

States:
- Focus:        Border Primary 500 (2px)
- Error:        Border Error Main, Helper text Error Main
- Disabled:     Background Gray 100, Border Gray 300

Label:
- Font:         Body Medium (14px)
- Color:        Gray 700
- Margin:       8px (하단)

Helper Text:
- Font:         Body Small (12px)
- Color:        Gray 600 (normal), Error Main (error)
- Margin:       8px (상단)
```

### 7.3 Card

```
Background:     Surface (#FFFFFF)
Border Radius:  12px
Padding:        24px
Elevation:      Level 1 (rest), Level 2 (hover)
Gap (내부):     16px

States:
- Hover:        Elevation Level 2
- Pressed:      Elevation Level 1 + Overlay Light
```

### 7.4 App Bar (상단 바)

```
Background:     Surface (#FFFFFF)
Height:         56px (Mobile), 64px (Tablet+)
Elevation:      Level 3
Padding:        0 16px

Title:
- Font:         Title Large (22px, SemiBold 600)
- Color:        Gray 900
- Alignment:    Left (default) / Center (선택)

Action Icons:
- Size:         24px
- Color:        Gray 700
- Padding:      12px (터치 영역 48x48)
```

### 7.5 Bottom Navigation Bar

```
Background:     Surface (#FFFFFF)
Height:         80px (56px + 24px Safe Area)
Elevation:      Level 3

Tab Item:
- Icon Size:    24px
- Label Font:   Label Small (11px, Medium 500)
- Padding:      8px (상단), 4px (아이콘-레이블 gap)
- Min Width:    80px

States:
- Active:       Icon & Label Primary 500
- Inactive:     Icon & Label Gray 600
- Hover:        Background Overlay Light
```

### 7.6 Chip / Tag

```
Background:     Gray 100 (default), Primary 100 (selected)
Text Color:     Gray 800 (default), Primary 700 (selected)
Font:           Label Medium (12px, Medium 500)
Padding:        8px 12px
Border Radius:  4px
Height:         32px

With Icon:
- Icon Size:    16px
- Gap:          4px

States:
- Hover:        Gray 200 (default), Primary 200 (selected)
- Pressed:      Gray 300 (default), Primary 300 (selected)
```

### 7.7 Dialog / Modal

```
Background:     Surface (#FFFFFF)
Border Radius:  16px
Elevation:      Level 5
Max Width:      560px (Mobile: 90% screen width)
Padding:        24px

Scrim:          rgba(0, 0, 0, 0.32)

Title:
- Font:         Headline Small (24px, SemiBold 600)
- Color:        Gray 900
- Margin:       0 0 16px 0

Content:
- Font:         Body Large (16px, Regular 400)
- Color:        Gray 700
- Margin:       0 0 24px 0

Actions:
- Alignment:    Right
- Gap:          8px
- Padding:      0 (top), 0 (bottom)
```

### 7.8 Bottom Sheet

```
Background:     Surface (#FFFFFF)
Border Radius:  16px (상단만)
Elevation:      Level 4
Padding:        24px (좌우), 24px (상단), 32px (하단) + Safe Area
Max Height:     90% screen height

Handle (선택):
- Width:        32px
- Height:       4px
- Color:        Gray 400
- Border Radius: 2px
- Margin:       8px (상단)
```

### 7.9 Snackbar / Toast

```
Background:     Gray 900 (alpha 90%)
Text Color:     White
Font:           Body Medium (14px, Regular 400)
Padding:        16px 24px
Border Radius:  4px
Elevation:      Level 3
Max Width:      560px
Min Height:     48px

Position:       Bottom center
Margin:         16px (하단) + Safe Area

Action Button:
- Text Color:   Accent 300
- Font:         Label Large (14px, Medium 500)
```

### 7.10 List Item

```
Background:     Surface (#FFFFFF)
Padding:        16px (수평), 12px (수직)
Min Height:     56px
Gap:            12px (leading-content, content-trailing)

States:
- Hover:        Background Gray 50
- Pressed:      Background Gray 100
- Selected:     Background Primary 50

Leading Icon:
- Size:         24px
- Color:        Gray 700
- Margin:       0 (right)

Title:
- Font:         Body Large (16px, Regular 400)
- Color:        Gray 900

Subtitle:
- Font:         Body Medium (14px, Regular 400)
- Color:        Gray 600
- Margin:       4px (상단)

Trailing Icon:
- Size:         24px
- Color:        Gray 600
```

### 7.11 Progress Indicator

#### Linear Progress
```
Height:         4px
Background:     Primary 100
Foreground:     Primary 500
Border Radius:  2px
```

#### Circular Progress
```
Size:           24px (Small), 40px (Medium), 56px (Large)
Stroke Width:   4px
Color:          Primary 500
```

### 7.12 Skeleton Loader

```
Background:     Gray 100
Animated Color: Gray 200
Border Radius:  컴포넌트에 맞춤 (4px ~ 12px)
Animation:      Shimmer (1.5s infinite)
```

---

## 8. 레이아웃 가이드

### 8.1 Grid System

```
Columns:        12 (Desktop), 8 (Tablet), 4 (Mobile)
Gutter:         16px (Mobile), 24px (Tablet), 24px (Desktop)
Margin:         16px (Mobile), 24px (Tablet), 24px (Desktop)
```

### 8.2 Breakpoints

```
Mobile (xs):    0px - 599px
Tablet (sm):    600px - 904px
Desktop (md):   905px - 1239px
Large (lg):     1240px - 1439px
XLarge (xl):    1440px+
```

### 8.3 Safe Area

```
iOS:
- Top:          Status Bar (44px ~ 47px)
- Bottom:       Home Indicator (34px)

Android:
- Top:          Status Bar (24px ~ 32px)
- Bottom:       Navigation Bar (48px, 선택)

권장: Flutter SafeArea 위젯 사용
```

---

## 9. 애니메이션 & Transition

### 9.1 Duration (지속 시간)

```
Instant:    0ms     - 즉시 (사용 제한)
Fast:       100ms   - 매우 빠름 (Ripple, Hover)
Quick:      200ms   - 빠름 (Fade, Scale) ★ 기본
Normal:     300ms   - 보통 (Slide, Transform)
Moderate:   400ms   - 중간 (Page transition)
Slow:       500ms   - 느림 (Complex animation)
```

### 9.2 Easing (가속도)

```
Standard:       cubic-bezier(0.4, 0.0, 0.2, 1)  - 기본
Decelerate:     cubic-bezier(0.0, 0.0, 0.2, 1)  - 진입
Accelerate:     cubic-bezier(0.4, 0.0, 1, 1)    - 퇴장
Emphasized:     cubic-bezier(0.2, 0.0, 0, 1)    - 강조
```

### 9.3 주요 애니메이션

#### Fade
```
Duration:   200ms
Easing:     Standard
Opacity:    0 → 1 (in), 1 → 0 (out)
```

#### Scale
```
Duration:   200ms
Easing:     Emphasized
Scale:      0.8 → 1 (in), 1 → 0.8 (out)
```

#### Slide
```
Duration:   300ms
Easing:     Standard
Offset:     화면 너비 기준
```

#### Ripple
```
Duration:   100ms (fade in), 200ms (fade out)
Easing:     Decelerate
Color:      Overlay Light
```

---

## 10. 접근성 (Accessibility)

### 10.1 색상 대비

**WCAG AA 기준 준수 (4.5:1 이상)**
```
Text (일반):           4.5:1 이상
Text (큰 텍스트):     3:1 이상
UI 컴포넌트:          3:1 이상
```

### 10.2 터치 타겟

```
최소 크기:   44x44 pt (iOS), 48x48 dp (Android)
권장 크기:   48x48 dp (모든 플랫폼)
최소 간격:   8px
```

### 10.3 Screen Reader

```
- 모든 인터랙티브 요소에 Semantic label 제공
- 이미지에 대체 텍스트 제공
- 폼 필드에 명확한 레이블 제공
- 에러 메시지를 screen reader가 읽을 수 있도록 구현
```

### 10.4 키보드 내비게이션

```
- Tab으로 모든 인터랙티브 요소 접근 가능
- Focus 상태 명확히 표시
- 논리적인 탭 순서 유지
```

---

## 11. 다크 모드 (향후 지원)

현재 MVP에서는 라이트 모드만 지원하며, 향후 다크 모드 추가 예정.

### 11.1 다크 모드 색상 (예비)

```
Background:        #121212
Surface:           #1E1E1E
Surface Variant:   #2C2C2C

Text Primary:      #FFFFFF
Text Secondary:    #B0B0B0
Text Disabled:     #6C6C6C

Primary:           #7986CB  (Primary 400)
Secondary:         #4DB6AC  (Secondary 400)
```

---

## 12. 플랫폼별 고려사항

### 12.1 iOS

```
- SF Symbols 사용 가능
- Cupertino 스타일 일부 차용 (Navigation, Modal)
- Safe Area 필수 고려
- Haptic Feedback 활용
```

### 12.2 Android

```
- Material Design 3 충실히 따름
- Ripple effect 기본 적용
- FAB, Snackbar 적극 활용
- System Navigation Bar 고려
```

---

## 13. 디자인 토큰 (Design Tokens)

디자인 시스템의 모든 값을 코드에서 재사용 가능한 토큰으로 정의합니다.

Flutter 구현 시 다음 위치에 정의:
```
lib/core/theme/
  - colors.dart
  - typography.dart
  - spacing.dart
  - shadows.dart
  - borders.dart
```

---

## 14. 브랜드 가이드라인

### 14.1 로고 (추후 정의)

현재 로고 미정. 텍스트 로고 "CareerMap" 사용.

### 14.2 톤 & 매너

**커뮤니케이션 원칙:**
- 전문적이지만 친근하게
- 명확하고 간결하게
- 긍정적이고 격려하는 톤
- 존댓말 사용

**예시:**
- ✅ "OO님에게 맞는 추천 5개가 업데이트됐어요"
- ✅ "이력을 더 입력하면 추천 정확도가 높아져요"
- ❌ "추천이 없습니다" (너무 무뚝뚝)
- ❌ "지금 바로 이력을 입력하세요!" (너무 강압적)

---

## 15. 참고 리소스

### 15.1 Material Design 3
- https://m3.material.io/

### 15.2 Pretendard Font
- https://github.com/orioncactus/pretendard

### 15.3 Material Symbols
- https://fonts.google.com/icons

---

## 변경 이력

| 버전 | 날짜 | 변경 내용 | 작성자 |
|------|------|-----------|--------|
| 1.0  | 2025-12-19 | 초안 작성 | Frontend Agent |

---

**Last Updated:** 2025-12-19
**Version:** 1.0
**Status:** Draft → Review Pending
