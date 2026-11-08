# Notices 스펙

- 경로: `/notices`
- 구현 파일: `src/ui/screens/NoticesScreen.tsx`
- 관련 API: [api/notices.md](../../api/notices.md)

## 1. 목적

앱 내 공지사항 관리 (작성·수정·삭제·노출 토글).

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 테이블 | 제목 · 내용(말줄임) · 노출 뱃지 · 작성일 · 수정일 · 액션 |
| 작성 버튼 | "+ 작성" 클릭 → 모달 오픈 |
| 액션 버튼 | 수정 / 숨기기·노출 토글 / 삭제 |
| 모달 | 제목·내용 입력, 저장·취소 |

### Notice 필드

| 필드 | 타입 | 설명 |
|---|---|---|
| `id` | string | 공지 ID |
| `title` | string | 제목 |
| `body` | string | 내용 |
| `active` | boolean | 앱 노출 여부 |
| `createdAt` | string | 작성일 |
| `updatedAt` | string | 수정일 |

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 목록 조회 | GET | `/api/admin/notices` | `{ notices: Notice[] }` |
| 공지 작성 | POST | `/api/admin/notices` | `{ title, body }` |
| 공지 수정 | PATCH | `/api/admin/notices/:id` | `{ title?, body?, active? }` |
| 공지 삭제 | DELETE | `/api/admin/notices/:id` | 삭제 전 confirm |

## 4. 완료 기준

- [x] 목록 조회
- [x] 공지 작성 (모달)
- [x] 공지 수정 (모달)
- [x] 노출 토글 (`active` PATCH)
- [x] 공지 삭제 (confirm 후 삭제)
- [x] 에러 상태 처리
