# 신고 관리 스펙

- 경로: `/reports`
- 구현 파일: `src/ui/screens/ReportsScreen.tsx`
- 관련 API: [api/reports.md](../../api/reports.md)

## 1. 목적

유저 신고 목록 확인 및 처리. 처리 시 신고 대상 유저 자동 제한 옵션.

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 필터 탭 | OPEN (미처리) / ACTIONED (처리됨) 토글 |
| 테이블 | 신고자 · 피신고자 · 신고 유형 · 내용 · 날짜 |
| 슬라이드 패널 | 신고 상세 + 처리 UI |

### 슬라이드 패널

- 신고 상세: 신고자/피신고자 정보, 신고 유형, 내용
- 처리 옵션:
  - 처리 시 자동 제한 (`withRestrict` 체크박스)
  - 제한 기간 선택: 1일 / 3일 / 7일 / 30일
- 처리 액션: `PATCH /api/admin/reports/:id` → status: "ACTIONED"
- 자동 제한 시: `PATCH /api/admin/users/:userId` → `restrictedUntil` 함께 전송

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 목록 조회 | GET | `/api/admin/reports?status=` | OPEN/ACTIONED 필터 |
| 신고 처리 | PATCH | `/api/admin/reports/:id` | `{ status: "ACTIONED" }` |
| 유저 제한 | PATCH | `/api/admin/users/:id` | `{ restrictedUntil }` |

## 4. 완료 기준

- [x] 목록 조회 (OPEN/ACTIONED 필터)
- [x] 슬라이드 패널
- [x] 신고 처리
- [x] 처리 시 자동 제한 옵션
- [x] 에러 상태 처리
- [ ] 메시지 신고 연결 — `messageId` 있는 신고에서 "메시지 보기" 버튼 → 채팅 화면 이동 — **신규** (Report 모델에 `messageId` 추가 필요)
