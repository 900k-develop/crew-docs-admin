# Activity Log 스펙

- 경로: `/activity-log`
- 구현 파일: `src/ui/screens/ActivityLogScreen.tsx`
- 관련 API: [api/activity-logs.md](../../api/activity-logs.md)

## 1. 목적

관리자 액션 감사 기록 열람. 누가 언제 어떤 대상에 무엇을 했는지 추적.

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 헤더 | "운영 로그" + 총 건수 |
| 액션 필터 | 전체/개별 액션 셀렉트박스 |
| 테이블 | 일시 · 관리자 · 액션 뱃지 · 대상 · 상세 |
| 페이지네이션 | PAGE_SIZE: 30 |

### 액션 타입 (현재 구현)

| action | 레이블 | 비고 |
|---|---|---|
| `USER_RESTRICT` | 유저 제한 | |
| `USER_UNRESTRICT` | 제한 해제 | |
| `USER_DELETE` | 유저 삭제 | |
| `REPORT_ACTION` | 신고 처리 | |
| `MEETUP_CANCEL` | 번개 취소 | |
| `GROUP_PREMIUM` | PREMIUM 부여 | |
| `GROUP_BASIC` | BASIC 전환 | |
| `NOTICE_CREATE` | 공지 작성 | |
| `NOTICE_UPDATE` | 공지 수정 | |
| `NOTICE_DELETE` | 공지 삭제 | |
| `SETTINGS_UPDATE` | 앱 설정 변경 | |
| `PUSH_SEND` | 푸시 발송 | |
| `MESSAGE_DELETE` | 메시지 삭제 | **신규** |
| `GROUP_MEMBER_KICK` | 크루 멤버 강퇴 | **신규** |
| `MEETUP_EDIT` | 번개 수정 | **신규** |

### 대상 타입

`USER`, `MEETUP`, `GROUP`, `NOTICE`, `SETTINGS`, `PUSH`, `MESSAGE`(신규)

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 목록 조회 | GET | `/api/admin/activity-logs?page=&action=` | action 없으면 전체 |

**응답 필드**

| 필드 | 타입 | 설명 |
|---|---|---|
| `logs` | `Log[]` | 로그 목록 |
| `total` | `number` | 전체 건수 |

**Log 필드**: `id`, `adminName`, `action`, `targetType`, `targetId`, `targetName`, `detail`, `createdAt`

## 4. 완료 기준

- [x] 목록 조회 (페이지네이션, 액션 필터)
- [x] 액션 뱃지 (action별 색상)
- [x] 대상 타입 한글 레이블
- [x] 에러 상태 처리 (ApiError + 재시도)
- [ ] `MESSAGE_DELETE` 액션 타입 — 채팅 메시지 삭제 기록 — **신규** (서버·프론트 추가 필요)
- [ ] `GROUP_MEMBER_KICK` 액션 타입 — 크루 멤버 강퇴 기록 — **신규** (서버·프론트 추가 필요)
- [ ] `MEETUP_EDIT` 액션 타입 — 번개 직접 수정 기록 — **신규** (서버·프론트 추가 필요)
