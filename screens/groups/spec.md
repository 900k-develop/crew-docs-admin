# 크루 관리 스펙

- 경로: `/groups`
- 구현 파일: `src/ui/screens/GroupsScreen.tsx`
- 관련 기획: [crew-docs PR #27](https://github.com/900k-develop/crew-docs/pull/27)

## 1. 목적

전체 크루 목록 조회, 멤버 관리, 강퇴, 크루 삭제.

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 검색창 | 크루명 검색 |
| 테이블 | 크루명 · 오너 · 멤버 수 · 카테고리 · 생성일 · 상태 |
| 슬라이드 패널 | 행 클릭 시 오른쪽에서 슬라이드 오픈 |
| 페이지네이션 | 20건/페이지 |

### 슬라이드 패널

- 기본 정보 (크루명, 카테고리, 소개, 오너, 생성일)
- 멤버 목록 (이름 · 역할 · 가입일 · 강퇴 버튼)
- 크루 번개 목록 (최근 5건)
- 액션:
  - 멤버 강퇴 (`DELETE /api/admin/groups/:id/members/:userId`)
  - 크루 삭제 (연결된 번개 일괄 취소 처리)

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 목록 조회 | GET | `/api/admin/groups?page=&q=` | |
| 상세 조회 | GET | `/api/admin/groups/:id` | 멤버·번개 포함 |
| 멤버 강퇴 | DELETE | `/api/admin/groups/:id/members/:userId` | |
| 크루 삭제 | DELETE | `/api/admin/groups/:id` | 번개 일괄 취소 |

## 4. 완료 기준

- [ ] 목록 조회
- [ ] 검색
- [ ] 슬라이드 패널: 기본 정보
- [ ] 슬라이드 패널: 멤버 목록 + 강퇴
- [ ] 크루 삭제
