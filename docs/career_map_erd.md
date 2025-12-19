1. API 명세 (v1)
   1.0 공통
   Base

Base URL: /v1

Auth: Authorization: Bearer <accessToken>

Content-Type: application/json

공통 응답 형식

에러는 다음 공통 포맷 권장:

{
"error": {
"code": "VALIDATION_ERROR",
"message": "Field is invalid",
"fields": [
{ "path": "email", "reason": "INVALID_FORMAT" }
],
"traceId": "b3f1c2..."
}
}

페이지네이션

GET 리스트는 cursor 기반 권장

{
"items": [],
"nextCursor": "eyJpZCI6..."
}

1.1 인증/회원가입
POST /auth/signup

회원가입

Request

{
"email": "name@example.com",
"password": "password1234",
"nickname": "minwoo",
"agreements": {
"terms": true,
"privacy": true,
"marketing": false
}
}

Response 201

{
"user": {
"id": "uuid",
"email": "name@example.com",
"nickname": "minwoo",
"createdAt": "2025-12-19T01:00:00Z"
},
"tokens": {
"accessToken": "jwt",
"refreshToken": "jwt",
"expiresInSec": 3600
}
}

POST /auth/login

Request

{
"email": "name@example.com",
"password": "password1234"
}

Response 200

{
"user": {
"id": "uuid",
"email": "name@example.com",
"nickname": "minwoo"
},
"tokens": {
"accessToken": "jwt",
"refreshToken": "jwt",
"expiresInSec": 3600
}
}

POST /auth/refresh

Request

{
"refreshToken": "jwt"
}

Response 200

{
"accessToken": "jwt",
"refreshToken": "jwt",
"expiresInSec": 3600
}

POST /auth/password/reset/request

재설정 메일 발송

Request

{ "email": "name@example.com" }

Response 200

{ "ok": true }

POST /auth/logout

(서버에서 refreshToken 폐기)

Request

{ "refreshToken": "jwt" }

Response 200

{ "ok": true }

1.2 온보딩/프로필(이력 요약)
GET /me

내 기본 정보

Response 200

{
"user": {
"id": "uuid",
"email": "name@example.com",
"nickname": "minwoo"
},
"profile": {
"currentRole": "Flutter Developer",
"totalMonths": 36,
"location": "Seoul",
"workStyle": "HYBRID",
"desiredRoles": ["Mobile Developer"],
"industries": ["Fintech"],
"salaryRange": { "min": 50000000, "max": 80000000 },
"companySize": ["MID", "ENTERPRISE"],
"resumeCompletion": 72
},
"privacy": {
"profileVisibility": "FRIENDS",
"moveVisibility": "FRIENDS",
"maskCompanyNameInMoves": false
}
}

PUT /me/profile

온보딩 폼/프로필 수정

Request

{
"currentRole": "Flutter Developer",
"totalMonths": 36,
"location": "Seoul",
"workStyle": "HYBRID",
"desiredRoles": ["Mobile Developer"],
"industries": ["Fintech"],
"salaryRange": { "min": 50000000, "max": 80000000 },
"companySize": ["MID", "ENTERPRISE"]
}

Response 200

{ "ok": true }

PUT /me/privacy

공개 범위 설정

Request

{
"profileVisibility": "FRIENDS",
"moveVisibility": "FRIENDS",
"maskCompanyNameInMoves": false
}

Response 200

{ "ok": true }

1.3 이력: 경력/프로젝트/스킬
GET /me/experiences?cursor=

경력 리스트

Response 200

{
"items": [
{
"id": "uuid",
"companyName": "Rowan",
"teamName": "Mobile",
"title": "Flutter Lead",
"employmentType": "FULL_TIME",
"startDate": "2023-01-01",
"endDate": null,
"isCurrent": true,
"highlights": [
"Built fintech payment flows",
"Led 3 devs across releases"
],
"domainTags": ["Fintech", "Healthcare"]
}
],
"nextCursor": null
}

POST /me/experiences

경력 추가

Request

{
"companyName": "Rowan",
"teamName": "Mobile",
"title": "Flutter Lead",
"employmentType": "FULL_TIME",
"startDate": "2023-01-01",
"endDate": null,
"isCurrent": true,
"highlights": ["...", "..."],
"domainTags": ["Fintech"]
}

Response 201

{ "id": "uuid" }

PUT /me/experiences/{experienceId}

Request

{
"teamName": "Mobile Part",
"highlights": ["Updated highlight 1", "Updated highlight 2"]
}

Response 200

{ "ok": true }

DELETE /me/experiences/{experienceId}

Response 200

{ "ok": true }

GET /me/projects?cursor=

Response 200

{
"items": [
{
"id": "uuid",
"name": "Konda App",
"role": "Lead",
"startDate": "2024-01-01",
"endDate": null,
"contribution": "Designed gRPC architecture",
"impactMetrics": [{ "label": "Crash-free", "value": "99.9%" }],
"skillTags": ["Flutter", "gRPC", "Riverpod"]
}
],
"nextCursor": null
}

POST /me/projects
{
"name": "Konda App",
"role": "Lead",
"startDate": "2024-01-01",
"endDate": null,
"contribution": "....",
"impactMetrics": [{ "label": "Revenue", "value": "+12%" }],
"skillTags": ["Flutter", "gRPC"]
}

Response 201

{ "id": "uuid" }

GET /me/skills

Response 200

{
"items": [
{ "skillId": "uuid", "name": "Flutter", "level": "ADVANCED" },
{ "skillId": "uuid", "name": "gRPC", "level": "INTERMEDIATE" }
]
}

PUT /me/skills

전체 교체(단순 운영에 유리)

Request

{
"items": [
{ "name": "Flutter", "level": "ADVANCED" },
{ "name": "Riverpod", "level": "ADVANCED" },
{ "name": "gRPC", "level": "INTERMEDIATE" }
]
}

Response 200

{ "ok": true }

GET /catalog/skills?q=flut

스킬 자동완성(사전)

Response 200

{
"items": [
{ "id": "uuid", "name": "Flutter" },
{ "id": "uuid", "name": "Flutter Hooks" }
]
}

1.4 추천(다음 직장 추천)
GET /recommendations?sort=FIT_DESC&cursor=&filters=

추천 리스트

Query 예시

desiredRole=Mobile%20Developer

location=Seoul

workStyle=REMOTE

industry=Fintech

Response 200

{
"items": [
{
"id": "uuid",
"company": { "id": "uuid", "name": "Danal" },
"roleTitle": "Flutter Developer",
"fitScore": 87,
"reasonSummary": "Your Flutter + fintech domain experience matches",
"createdAt": "2025-12-19T01:20:00Z",
"isSaved": false,
"isHidden": false
}
],
"nextCursor": null
}

GET /recommendations/{recommendationId}

추천 상세

Response 200

{
"id": "uuid",
"company": { "id": "uuid", "name": "Danal" },
"roleTitle": "Flutter Developer",
"fitScore": 87,
"reasons": [
{ "type": "SKILL_MATCH", "text": "Flutter + Riverpod matches core stack" },
{ "type": "DOMAIN_MATCH", "text": "Fintech/payment experience aligns" }
],
"gaps": [
{
"type": "GAP",
"name": "Mobile security hardening",
"priority": "HIGH",
"actions": [
{ "text": "Implement Play Integrity/App Attest validation flow" },
{ "text": "Add certificate pinning & jailbreak/root detection plan" }
]
}
],
"networkSignals": {
"visible": true,
"movedFriendsCount": 2,
"similarFriendsCount": 1
}
}

POST /recommendations/{recommendationId}/feedback

저장/숨김/관심/무관 피드백

Request

{
"action": "SAVE",
"meta": { "source": "LIST" }
}

Response 200

{ "ok": true }

1.5 네트워크(동료 추가/요청)
GET /network/friends?cursor=

친구 목록

Response 200

{
"items": [
{
"friendUserId": "uuid",
"nickname": "aiden",
"headline": "iOS Developer",
"lastMoveUpdatedAt": "2025-12-01T10:00:00Z",
"visibility": "FRIENDS"
}
],
"nextCursor": null
}

POST /network/invites

내 초대코드 생성/재생성(또는 최초 1회 생성 후 재사용)

Response 200

{
"inviteCode": "CM-7K2P9Q",
"expiresAt": "2026-01-19T00:00:00Z"
}

POST /network/requests

초대코드로 친구 요청

Request

{
"inviteCode": "CM-7K2P9Q",
"message": "같이 일했던 동료입니다!"
}

Response 201

{
"requestId": "uuid",
"status": "PENDING"
}

GET /network/requests?type=INCOMING|OUTGOING

요청 목록

Response 200

{
"items": [
{
"requestId": "uuid",
"fromUser": { "id": "uuid", "nickname": "minwoo" },
"toUser": { "id": "uuid", "nickname": "aiden" },
"status": "PENDING",
"createdAt": "2025-12-19T01:00:00Z"
}
]
}

POST /network/requests/{requestId}/accept

Response 200

{ "ok": true }

POST /network/requests/{requestId}/reject

Response 200

{ "ok": true }

POST /network/friends/{friendUserId}/block

Response 200

{ "ok": true }

1.6 이직 이벤트(내/동료)
GET /me/moves?cursor=

내 이직/이동 이벤트

Response 200

{
"items": [
{
"id": "uuid",
"type": "JOB_CHANGE",
"companyName": "Rowan",
"companyMasked": false,
"roleTitle": "Flutter Lead",
"eventDate": "2024-10-01",
"visibility": "FRIENDS",
"note": "Moved to fintech team"
}
],
"nextCursor": null
}

POST /me/moves

Request

{
"type": "JOB_CHANGE",
"companyName": "Rowan",
"roleTitle": "Flutter Lead",
"eventDate": "2024-10-01",
"visibility": "FRIENDS",
"note": "Moved to fintech team"
}

Response 201

{ "id": "uuid" }

GET /users/{userId}/moves?cursor=

친구의 이동 이벤트(권한 체크)

Response 200

{
"items": [
{
"id": "uuid",
"type": "JOB_CHANGE",
"companyName": "***",
"companyMasked": true,
"roleTitle": "Backend Engineer",
"eventDate": "2025-11-01"
}
],
"nextCursor": null
}

1.7 예측(네트워크 기반)
GET /predictions/companies?cursor=

가능성 높은 회사 Top N

Response 200

{
"generatedAt": "2025-12-19T01:30:00Z",
"items": [
{
"company": { "id": "uuid", "name": "Danal" },
"score": 0.82,
"tier": "HIGH",
"evidence": {
"movedFriendsCount": 4,
"recentMovesCount": 2,
"similarFriendsCount": 1
}
}
],
"nextCursor": null
}

GET /predictions/companies/{companyId}

회사별 예측 상세(근거 확장)

Response 200

{
"company": { "id": "uuid", "name": "Danal" },
"score": 0.82,
"signals": [
{ "type": "FRIEND_MOVES", "weight": 0.45, "value": 4 },
{ "type": "RECENCY", "weight": 0.20, "value": "6M" },
{ "type": "SIMILARITY", "weight": 0.35, "value": 0.71 }
],
"gaps": [
{ "name": "Payments compliance", "priority": "MEDIUM" }
]
}

POST /predictions/recompute

(선택) 사용자가 “업데이트” 눌렀을 때 재계산 트리거. 아니면 서버에서 배치로 갱신.

Response 202

{ "accepted": true }

1.8 설정/탈퇴
DELETE /me

탈퇴(데이터 삭제 정책에 맞춰 soft-delete 권장)

Response 200

{ "ok": true }

2. DB 스키마 초안(ERD)

아래는 MVP 필수 엔터티 중심으로 구성했어.

2.1 Mermaid ERD
erDiagram
users ||--|| user_profiles : has
users ||--|| user_privacy_settings : has
users ||--o{ user_skills : has
skills ||--o{ user_skills : used_by

users ||--o{ experiences : has
users ||--o{ projects : has
projects ||--o{ project_skills : has
skills ||--o{ project_skills : referenced_by

users ||--o{ move_events : has

users ||--o{ invite_codes : creates
users ||--o{ friend_requests : sends
users ||--o{ friend_requests : receives
users ||--o{ friendships : has
users ||--o{ friendships : has_peer

companies ||--o{ job_roles : has
users ||--o{ recommendations : receives
recommendations }o--|| companies : about
recommendations }o--|| job_roles : about
recommendations ||--o{ recommendation_feedback : has

users ||--o{ prediction_snapshots : has
prediction_snapshots ||--o{ predicted_companies : contains
predicted_companies }o--|| companies : about

users {
uuid id PK
string email UNIQUE
string password_hash
string nickname UNIQUE
string status
datetime created_at
datetime updated_at
datetime deleted_at
}

user_profiles {
uuid user_id PK,FK
string current_role
int total_months
string location
string work_style
string[] desired_roles
string[] industries
int salary_min
int salary_max
string[] company_size
int resume_completion
datetime updated_at
}

user_privacy_settings {
uuid user_id PK,FK
string profile_visibility
string move_visibility
boolean mask_company_name_in_moves
datetime updated_at
}

skills {
uuid id PK
string name UNIQUE
string category
datetime created_at
}

user_skills {
uuid user_id FK
uuid skill_id FK
string level
datetime updated_at
%% composite PK (user_id, skill_id)
}

experiences {
uuid id PK
uuid user_id FK
string company_name
string team_name
string title
string employment_type
date start_date
date end_date
boolean is_current
string[] highlights
string[] domain_tags
datetime created_at
datetime updated_at
}

projects {
uuid id PK
uuid user_id FK
string name
string role
date start_date
date end_date
string contribution
json impact_metrics
datetime created_at
datetime updated_at
}

project_skills {
uuid project_id FK
uuid skill_id FK
%% composite PK (project_id, skill_id)
}

move_events {
uuid id PK
uuid user_id FK
string type
string company_name
string role_title
date event_date
string visibility
boolean company_masked
string note
datetime created_at
datetime updated_at
}

invite_codes {
uuid id PK
uuid owner_user_id FK
string code UNIQUE
datetime expires_at
string status
datetime created_at
}

friend_requests {
uuid id PK
uuid from_user_id FK
uuid to_user_id FK
string status
string message
datetime created_at
datetime responded_at
}

friendships {
uuid id PK
uuid user_id FK
uuid friend_user_id FK
datetime created_at
string status
%% unique (user_id, friend_user_id)
}

companies {
uuid id PK
string name UNIQUE
string industry
string size
string location
datetime created_at
}

job_roles {
uuid id PK
uuid company_id FK
string title
string level
string work_style
datetime created_at
}

recommendations {
uuid id PK
uuid user_id FK
uuid company_id FK
uuid job_role_id FK
int fit_score
string reason_summary
json reasons
json gaps
boolean is_saved
boolean is_hidden
datetime created_at
datetime updated_at
}

recommendation_feedback {
uuid id PK
uuid recommendation_id FK
uuid user_id FK
string action
json meta
datetime created_at
}

prediction_snapshots {
uuid id PK
uuid user_id FK
string model_version
datetime generated_at
}

predicted_companies {
uuid id PK
uuid snapshot_id FK
uuid company_id FK
float score
string tier
json evidence
}

2.2 테이블 설계 메모(중요 포인트)
친구 관계 설계(권장)

friendships는 양방향 row를 2개 생성하는 방식이 앱 조회에 편함

A-B 수락 시:

(user_id=A, friend_user_id=B)

(user_id=B, friend_user_id=A)

공개 범위/마스킹

move_events.visibility + company_masked를 함께 사용

기본 마스킹은 user_privacy_settings.mask_company_name_in_moves로 처리 가능(조회 시 적용)

추천/예측은 “스냅샷”으로 저장

매번 계산하지 말고,

추천: recommendations에 최신 결과 저장(업데이트 시 재생성/업서트)

예측: prediction_snapshots + predicted_companies로 배치/트리거 계산 결과 저장

인덱스 추천(최소)

users(email) UNIQUE, users(nickname) UNIQUE

experiences(user_id, start_date desc)

projects(user_id, created_at desc)

move_events(user_id, event_date desc)

friend_requests(to_user_id, status), friend_requests(from_user_id, status)

friendships(user_id, friend_user_id) UNIQUE

recommendations(user_id, created_at desc), (user_id, is_saved), (user_id, is_hidden)

predicted_companies(snapshot_id, score desc)
