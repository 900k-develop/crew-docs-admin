# 크루 채팅 메시지 관리 스펙

- 경로: `/groups/:id/messages`
- 구현 파일: `src/ui/screens/GroupMessagesScreen.tsx`
- 관련 기획: [crew-docs PR #25](https://github.com/900k-develop/crew-docs/pull/25)
- **상태**: 크루 채팅 개발 확정 후 구현. [#42](https://github.com/900k-develop/crew-docs/issues/42)(출시 일정·백엔드 담당)가 정해지면 착수. 사람이 모자라면 크루 채팅부터 빠질 수 있음 ([개발 범위·일정](https://github.com/900k-develop/crew-docs/blob/main/docs/5-launch/mvp-schedule.md))
- 관련 화면: [groups/spec.md](../groups/spec.md)

## 1. 목적

크루 채팅 메시지 조회 및 신고된 메시지 처리 (삭제).

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 헤더 | 크루명 + 메시지 수 |
| 메시지 목록 | 발신자 · 내용 · 시간 · 신고 여부 · 삭제 버튼 |
| 필터 탭 | 전체 / 신고된 메시지만 |
| 페이지네이션 | 50건/페이지 |

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 메시지 목록 | GET | `/api/admin/groups/:id/messages?page=` | |
| 메시지 삭제 | DELETE | `/api/admin/groups/:groupId/messages/:messageId` | REST 경로 일관성 — 서버 착수 시 확정 |

> **신규 API 필요**: 크루 채팅은 서버 신규 개발 항목. API 엔드포인트는 개발 착수 시 확정.

## 4. 완료 기준

- [ ] 메시지 목록 조회
- [ ] 신고된 메시지 필터
- [ ] 메시지 삭제
