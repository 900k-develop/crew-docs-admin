# 크루 상세 스펙

- 경로: `/groups/:id`
- 구현 파일: `src/ui/screens/GroupDetailScreen.tsx`
- 관련 화면: [groups/spec.md](../groups/spec.md)
- 관련 기획: [crew-docs PR #27](https://github.com/900k-develop/crew-docs/pull/27)

## 1. 목적

크루 목록에서 행을 클릭하면 열리는 전용 상세 페이지. 운영팀이 한 크루의 전체 정보를 한눈에 보고, 멤버 관리·오너 변경·삭제 등 주요 운영 액션을 수행할 수 있게 한다.

> 현재 크루 목록에서 프리미엄 토글만 가능하고, 멤버 목록·오너 변경·삭제는 액션이 없는 상태 — 이 화면이 그 공백을 메운다.

## 2. 진입 경로

- 크루 목록(`/groups`) 행 클릭 → 이 페이지로 이동
- URL 직접 접근 가능 (`/groups/:id`)

## 3. UI 구성

### 헤더

| 영역 | 내용 |
|---|---|
| 크루 대표 이미지 | 없으면 플레이스홀더 |
| 크루명 | 카테고리 뱃지 함께 표시 |
| 등급 | BASIC / PREMIUM (만료일 포함) |
| 생성일 | |
| 뒤로가기 | 크루 목록으로 |

### 기본 정보 섹션

| 필드 | 설명 |
|---|---|
| 소개 | `description` |
| 동네 | `neighborhood` |
| 태그 | `tags[]` |
| 최대 멤버 수 | `maxMembers` (없으면 "제한 없음") |
| 가입 승인제 | `requireApproval` |
| 오너 | 이름 + 전화번호 (유저 상세 링크) |

### 멤버 목록 섹션

| 컬럼 | 내용 |
|---|---|
| 이름 | 클릭 시 `/users/:id` 이동 |
| 전화번호 | |
| 역할 | OWNER / MEMBER 뱃지 |
| 가입일 | |
| 액션 | 강퇴 버튼 (OWNER는 비활성) |

- 페이지네이션: 멤버 20건/페이지 (또는 전체 표시 — 멤버 수 100명 미만이면 전체)
- 오너 변경: 멤버 행의 "오너로 변경" 버튼 → 확인 모달 (현 오너 이름 표시 후 변경 확인)

### 번개 목록 섹션

최근 번개 최대 10건. 더 많으면 "번개 목록에서 보기(`/meetups?groupId=:id`)" 링크.

| 컬럼 | 내용 |
|---|---|
| 제목 | 클릭 시 `/meetups/:id` 이동 |
| 종류 | 번개 / 정모 |
| 상태 | 모집 중 / 확정 / 취소됨 |
| 시작일 | |
| 인원 | 참가/정원 |

### 운영 액션 (우측 상단 또는 하단 고정)

| 액션 | 설명 |
|---|---|
| 프리미엄 전환 / 해제 | 현재 `GroupsScreen`에 있는 토글을 이 화면으로 이전 |
| 크루 삭제 | 삭제 확인 모달 — "크루명" 직접 입력 후 활성화. 번개 일괄 취소(서버 cascade) 경고 표시 |

## 4. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 상세 조회 | GET | `/api/admin/groups/:id` | 멤버·번개 포함 — 현재 미구현 |
| 프리미엄 토글 | PATCH | `/api/admin/groups/:id` | `{ tier, premiumUntil }` — 기존 |
| 멤버 강퇴 | DELETE | `/api/admin/groups/:id/members/:userId` | 미구현 |
| 오너 변경 | PATCH | `/api/admin/groups/:id/owner` | `{ newOwnerId }` — 미구현 |
| 크루 삭제 | DELETE | `/api/admin/groups/:id` | 번개 cascade — 미구현 |

> `GET /api/admin/groups/:id` (상세), `DELETE .../members/:userId`, `PATCH .../owner`, `DELETE /api/admin/groups/:id` 4개 API 신규 개발 필요.

## 5. 완료 기준

- [ ] 상세 조회 (서버 API 개발 후)
- [ ] 기본 정보 표시
- [ ] 멤버 목록 + 강퇴
- [ ] 오너 변경 (확인 모달 포함)
- [ ] 번개 목록 (최근 10건)
- [ ] 프리미엄 토글 (GroupsScreen에서 이 화면으로 이전)
- [ ] 크루 삭제 (이름 입력 확인 모달)
