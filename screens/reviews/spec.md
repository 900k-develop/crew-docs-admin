# 후기 관리 스펙

- 경로: `/reviews`
- 구현 파일: `src/ui/screens/ReviewsScreen.tsx`
- 관련 기획: [crew-docs PR #30](https://github.com/900k-develop/crew-docs/pull/30) — 신뢰·후기 기획

## 1. 목적

번개 종료 후 작성된 후기 조회 및 신고된 후기 처리 (삭제).

> **서버 신규 개발 항목**: 후기 DB 모델·API 미구현. 어드민 화면은 서버 개발 완료 후 구현.

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 필터 탭 | 전체 / 신고된 후기만 |
| 테이블 | 작성자 · 대상(번개) · 별점 · 한 줄 내용 · 작성일 · 신고 여부 |
| 슬라이드 패널 | 후기 상세 + 삭제 처리 |

### 슬라이드 패널

- 후기 내용 전문
- 작성자 정보 (이름, 매너점수)
- 대상 번개 링크
- 삭제 버튼 (신고된 후기 처리)

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 목록 조회 | GET | `/api/admin/reviews?page=&reported=` | |
| 후기 삭제 | DELETE | `/api/admin/reviews/:id` | 별점 평균 재계산 필요 |

## 4. 신고 연결 방식

현재 `Report`는 `targetId`(유저) + `meetupId`(번개)만 가리킨다. 후기를 신고 대상으로 연결하려면 서버 스키마 변경이 필요하다.

> **권장 방식**: `Report`에 `targetType` (`USER` | `MEETUP` | `REVIEW`) + `targetId` 필드 일반화 ([신뢰 정책](https://github.com/900k-develop/crew-docs/blob/main/docs/3-policies/trust.md) 15 — "후기 한 줄도 신고할 수 있다"). 서버 개발 착수 시 `crew-api` 이슈로 연다.

## 5. 삭제 기준

- 후기 삭제는 매너온도에 영향 없음 ([신뢰 정책](https://github.com/900k-develop/crew-docs/blob/main/docs/3-policies/trust.md) 14)
- 삭제 시 호스트의 **별점 평균** 재계산 필요 (별점 3개 이상 표시 조건 — 서버에서 처리)
- 삭제 사유: 신고 처리, 허위·욕설 내용

## 6. 완료 기준

- [ ] 목록 조회 (서버 API 개발 후)
- [ ] 신고된 후기 필터
- [ ] 후기 삭제 (별점 평균 재계산 포함)
