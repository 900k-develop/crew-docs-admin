# Stats API

베이스 경로: `/api/admin/stats`

인증: `admin_session` 쿠키 필요

## GET /api/admin/stats

대시보드용 통계 조회.

**응답**

```json
{
  "stats": {
    "users": 12,
    "groups": 15,
    "meetups": 63,
    "openReports": 0,
    "activeMeetups": 5,
    "deltaUsers": 2,
    "deltaGroups": 0,
    "deltaMeetups": 3,
    "deltaReports": 0,
    "trend": [
      { "date": "2026-10-28", "users": 12, "meetups": 63 },
      { "date": "2026-10-29", "users": 0, "meetups": 0 }
    ]
  },
  "recentUsers": [
    {
      "id": "cuid...",
      "name": "홍길동",
      "phone": "01012345678",
      "mannerScore": 36.5,
      "neighborhood": "구래동",
      "createdAt": "2026-11-04T10:00:00.000Z"
    }
  ]
}
```

**필드 설명**

| 필드 | 설명 |
|---|---|
| `activeMeetups` | status OPEN 또는 CONFIRMED이고 startAt >= 현재 시각인 번개 수 |
| `delta*` | 오늘 00:00 이후 생성된 건수 |
| `trend` | 최근 7일 각 날짜의 신규 가입자 수, 번개 생성 수 |
| `recentUsers` | 최근 가입 5명 |
