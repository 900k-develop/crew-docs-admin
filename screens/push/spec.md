# Push 스펙

- 경로: `/push`
- 구현 파일: `src/ui/screens/PushScreen.tsx`

## 1. 목적

어드민이 앱 사용자에게 FCM 푸시 알림을 발송한다.  
잘못된 전체 발송은 되돌릴 수 없기 때문에 **3단계 안전장치**를 적용한다.

## 2. 발송 대상

| 모드 | 설명 |
|------|------|
| FCM 토큰 | 토큰 직접 입력 — 개발/테스트용 |
| 특정 유저 | 유저 ID 입력 → 해당 유저 1명 |
| 전체 유저 | 푸시 토큰이 등록된 전체 유저 |

## 3. 3단계 안전장치

### 1단계 — 미리보기 (UI)

- **"발송" 버튼이 없음.** 먼저 "대상 확인" 버튼을 눌러야 한다.
- `POST /api/admin/push/preview` 호출 → 실제 발송 없이 대상자 수만 반환.
- 미리보기 모달: 발송 대상, 제목, 내용, 대상자 수를 한눈에 확인.
- 대상자가 0명이면 최종 발송 버튼 비활성화.

### 2단계 — 텍스트 직접 입력 확인 (UI, 전체 발송 시만 적용)

- 전체 발송 선택 시 미리보기 모달에서 **`전송`** 텍스트를 직접 입력해야 최종 발송 버튼이 활성화.
- 오타·실수로 클릭되는 사고 방지.

### 3단계 — 서버 측 이중 차단 (API)

| 규칙 | 내용 |
|------|------|
| **하루 3회 제한** | 전체 발송(`target: "all"`)은 당일 최대 3회. 초과 시 HTTP 429 |
| **5분 내 중복 차단** | 동일 제목+내용이 5분 이내에 이미 전체 발송된 경우 HTTP 409 |
| **PushLog 기록** | 모든 발송(토큰/유저/전체)을 `PushLog` 테이블에 기록 |
| **ActivityLog 기록** | `PUSH_SEND` / `PUSH_SEND_ALL` 액션으로 어드민 활동 로그 저장 |

## 4. API

### POST /api/admin/push/preview
발송 없이 대상자 수 조회.

**Request**
```json
{ "target": "all" | "user" | "token", "userId": "...", "token": "..." }
```

**Response**
```json
{ "count": 1234, "name": "유저명(user 모드일 때)" }
```

### POST /api/admin/push
실제 발송.

**Request**
```json
{
  "target": "all" | "user" | "token",
  "token": "FCM토큰",
  "userId": "cuid",
  "type": "MEETUP_JOIN | ...",
  "title": "알림 제목",
  "body": "알림 내용",
  "data": { "meetupId": "...", "groupId": "..." }
}
```

**Response**
```json
{ "ok": true, "sent": 1234 }
```

**에러**
- `400`: title/body 누락, 유저 없음, 푸시 토큰 없음
- `409`: 5분 내 동일 내용 중복
- `429`: 하루 3회 초과

## 5. DB 모델

```prisma
model PushLog {
  id         String   @id @default(cuid())
  adminName  String
  target     String   // "token" | "user" | "all"
  targetId   String   @default("")
  title      String
  body       String
  sentCount  Int
  createdAt  DateTime @default(now())

  @@index([createdAt(sort: Desc)])
}
```

## 6. 완료 기준

- [x] 발송 대상 선택 (FCM 토큰 / 특정 유저 / 전체 유저)
- [x] 미리보기 — 발송 전 대상자 수 확인 필수 (1단계)
- [x] 전체 발송 시 '전송' 텍스트 입력 확인 (2단계)
- [x] 하루 3회 전체발송 제한 (3단계)
- [x] 5분 내 중복발송 차단 (3단계)
- [x] PushLog 발송 이력 저장
- [x] ActivityLog 어드민 활동 기록
- [x] 발송 완료 후 결과(발송 수) 표시
- [x] 에러 상태 표시 (대상자 없음, rate limit 초과 등)
