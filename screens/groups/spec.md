# Groups 스펙

- 경로: `/groups`
- 구현 파일: `src/ui/screens/UgroupsScreen.tsx`

## 1. 목적

전체 크루 목록 조회·검색, 등급 변경, 멤버 관리(강퇴).

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 검색창 | 크루 이름 검색 |
| 테이블 | 이름 · 카테고리 · 동네 · 등급 · 멤버수 · 생성일 |
| 슬라이드 패널 | 행 클릭 시 오른쪽에서 슬라이드 오픈 |
| 페이지네이션 | 페이지 단위 (PAGE_SIZE: 20) |

### 슬라이드 패널

- 기본 정보 (이름, 카테고리, 동네, 등급, premiumUntil, 멤버수)
- 등급 변경: BASIC ↔ PREMIUM, premiumUntil 날짜 선택
- 멤버 목록 탭: 멤버 이름·가입일 목록, 각 행에 강퇴 버튼

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 목록 조회 | GET | `/api/admin/groups?page=&q=` | |
| 등급 변경 | PATCH | `/api/admin/groups/:id` | `{ tier, premiumUntil? }` |
| 멤버 목록 | GET | `/api/admin/groups/:id/members` | **신규 API** |
| 멤버 강퇴 | DELETE | `/api/admin/groups/:id/members/:userId` | **신규 API** |

## 4. 완료 기준

- [x] 목록 조회
- [x] 검색
- [x] 등급 변경
- [ ] 슬라이드 패널
- [ ] 멤버 목록 탭
- [ ] 멤버 강퇴
