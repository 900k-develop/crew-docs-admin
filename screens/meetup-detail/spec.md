# 번개 상세 스펙

- 경로: `/meetups/:id`
- 구현 파일: `src/ui/screens/MeetupDetailScreen.tsx`
- 관련 화면: [meetups/spec.md](../meetups/spec.md)
- 관련 기획: [crew-docs lightning.md](https://github.com/900k-develop/crew-docs/blob/main/docs/2-drafts/lightning.md)

## 1. 목적

번개 목록에서 행을 클릭하면 열리는 전용 상세 페이지. 운영팀이 번개 상세 정보·참가자 목록을 한눈에 확인하고, 취소·강제 확정·참가자 관리 등 운영 액션을 수행할 수 있게 한다.

> 현재 번개 목록에서 "취소" 버튼만 있고, 참가자 목록·노쇼 처리·개별 강퇴는 불가 — 이 화면이 그 공백을 메운다.

## 2. 진입 경로

- 번개 목록(`/meetups`) 행 클릭 → 이 페이지로 이동
- 크루 상세(`/groups/:id`) 번개 목록 행 클릭
- URL 직접 접근 가능 (`/meetups/:id`)

## 3. UI 구성

### 헤더

| 영역 | 내용 |
|---|---|
| 번개 커버 이미지 | 없으면 플레이스홀더 |
| 제목 | 종류 뱃지(번개/정모) + 상태 뱃지 |
| 호스트 이름 | 클릭 시 `/users/:id` 이동 |
| 시작일시 | |
| 뒤로가기 | 번개 목록으로 |

### 기본 정보 섹션

| 필드 | 설명 |
|---|---|
| 소개 | `intro` |
| 장소 | `placeName` + `placeAddress` |
| 카테고리 | |
| 태그 | `tags[]` |
| 정원 / 최소 인원 | `capacity` / `minParticipants` |
| 참가비 | `feeWon`원 (0이면 "무료") |
| 가입 승인제 | `requireApproval` |
| 연결 크루 | `groupId`가 있으면 크루명 + `/groups/:id` 링크 |
| 준비물 | `prepItems[]` |
| 진행 타임라인 | `agenda[]` |

### 참가자 목록 섹션

| 컬럼 | 내용 |
|---|---|
| 이름 | 클릭 시 `/users/:id` 이동 |
| 전화번호 | |
| 상태 | JOINED / CANCELED / NO_SHOW 뱃지 |
| 참가 신청일 | |
| 액션 | 강퇴 버튼 (CANCELED·NO_SHOW는 비활성) |

- JOINED 인원이 `minParticipants` 미만이면 상단에 경고 배너: "최소 인원 미달 — 번개 성사 조건 미충족"

### 운영 액션

| 액션 | 조건 | 설명 |
|---|---|---|
| 번개 취소 | 상태가 CANCELED가 아닐 때 | 확인 모달. 참가자 전원 CANCELED 처리, 알림 발송은 서버 처리 |
| 강제 확정 | 상태 OPEN일 때 | 최소 인원 미달이어도 CONFIRMED로 변경. 사유 입력 필수 |
| 참가자 강퇴 | 참가자 목록 행 액션 | JOINED → CANCELED 처리 |
| 노쇼 처리 | 번개 종료 후 (`startAt` 경과) | 개별 참가자 NO_SHOW 표시 — 서버에서 매너온도 차감 ([신뢰 정책](https://github.com/900k-develop/crew-docs/blob/main/docs/3-policies/trust.md) 참고) |

## 4. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 상세 조회 | GET | `/api/admin/meetups/:id` | 참가자 목록 포함 — 미구현 |
| 번개 취소 | PATCH | `/api/admin/meetups/:id` | `{ status: "CANCELED" }` — 기존 |
| 강제 확정 | PATCH | `/api/admin/meetups/:id` | `{ status: "CONFIRMED", reason }` — 미구현 |
| 참가자 강퇴 | DELETE | `/api/admin/meetups/:id/attendances/:userId` | 미구현 |
| 노쇼 처리 | PATCH | `/api/admin/meetups/:id/attendances/:userId` | `{ status: "NO_SHOW" }` — 미구현 |

> `GET /api/admin/meetups/:id` (상세+참가자), `DELETE .../attendances/:userId`, `PATCH .../attendances/:userId`, `PATCH`에 `reason` 필드 4개 신규 개발 필요.

## 5. 완료 기준

- [ ] 상세 조회 (서버 API 개발 후)
- [ ] 기본 정보 표시
- [ ] 참가자 목록 + 강퇴
- [ ] 번개 취소 (MeetupsScreen에서 이 화면으로 이전)
- [ ] 강제 확정 (사유 입력 모달)
- [ ] 노쇼 처리 (번개 종료 후만 표시)
- [ ] 최소 인원 미달 경고 배너
