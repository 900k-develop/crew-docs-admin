# 대시보드 스펙

- 경로: `/dashboard`
- 구현 파일: `src/ui/screens/DashboardScreen.tsx`
- 관련 API: [api/stats.md](../../api/stats.md)

## 1. 목적

Crew 서비스 현황을 한눈에 확인. 로그인 후 첫 화면.

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| StatCard ×5 | 전체 유저, 전체 크루, 전체 번개, 미처리 신고, 진행 중 번개 |
| 스파크라인 | 유저·번개 7일 트렌드 미니 차트 (StatCard 내) |
| 델타 뱃지 | 전일 대비 증감 (▲/▼) |
| 7일 바 차트 ×2 | 신규 가입 / 번개 생성 (일별 BarRow) |
| 최근 가입 유저 | 최근 5명 테이블 |

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 응답 필드 |
|---|---|---|---|
| 통계 조회 | GET | `/api/admin/stats` | `stats`, `recentUsers` |

**stats 응답 필드**

| 필드 | 설명 |
|---|---|
| `users` | 전체 유저 수 |
| `groups` | 전체 크루 수 |
| `meetups` | 전체 번개 수 |
| `openReports` | 미처리 신고 수 |
| `activeMeetups` | 진행 예정 번개 수 (status OPEN/CONFIRMED, startAt >= now) |
| `deltaUsers` | 오늘 신규 가입 |
| `deltaGroups` | 오늘 신규 크루 |
| `deltaMeetups` | 오늘 신규 번개 |
| `deltaReports` | 오늘 신규 신고 |
| `trend` | 최근 7일 `{ date, users, meetups }[]` |

## 4. 완료 기준

- [x] StatCard 5종 렌더
- [x] 스파크라인 (데이터 없으면 null)
- [x] 델타 뱃지
- [x] 7일 바 차트
- [x] 최근 가입 유저 테이블
- [x] 에러 상태 처리 (ApiError)
- [ ] 채팅 활성도 StatCard 2종 — 오늘 발송 메시지 수, 활성 채팅방 수 — **신규** (stats API 확장 필요)
