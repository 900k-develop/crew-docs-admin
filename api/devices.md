# Devices API

베이스 경로: `/api/devices`

인증: 유저 세션 필요 (어드민 아님 — 앱에서 호출)

## POST /api/devices

기기 등록 또는 갱신. fcmToken unique 기준 upsert.

**요청**

```json
{
  "os": "ios",
  "osVersion": "17.4",
  "appVersion": "1.0.0",
  "model": "iPhone",
  "fcmToken": "dGAkEqQ1SxiXvHsT6VCofG:APA91bHy..."
}
```

**응답**

```json
{ "ok": true }
```

**동작**

1. `fcmToken`이 이미 존재하면 `osVersion`, `appVersion`, `lastActiveAt` 갱신
2. 없으면 새 `Device` 레코드 생성
3. 세션의 `userId`와 연결

---

## GET /api/admin/users/:id (확장)

유저 상세 조회 — `devices` 배열 포함.

```json
{
  "user": { ... },
  "devices": [
    {
      "id": "cuid...",
      "os": "android",
      "osVersion": "14",
      "appVersion": "1.0.0",
      "model": "samsung SM-S921N",
      "fcmToken": "eaxGUmNmS9...",
      "lastActiveAt": "2026-11-04T10:00:00.000Z",
      "createdAt": "2026-09-03T12:44:27.000Z"
    }
  ]
}
```

## 현황

- [ ] `Device` Prisma 모델 추가
- [ ] `POST /api/devices` 엔드포인트
- [ ] `GET /api/admin/users/:id` devices 포함
