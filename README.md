# crew-docs-admin

Crew 서비스 관리자 패널(crew-admin)의 화면 스펙 · API · 데이터 모델 문서 저장소.

## 작성 규칙

- 문서는 일반 마크다운(`.md`)으로 쓴다.
- 링크는 **일반 마크다운 상대 경로**만 쓴다. `[[위키링크]]`는 쓰지 않는다.
- 파일 이름은 영문 소문자 kebab-case (`user-detail.md`).
- 화면 스펙은 대응하는 API 엔드포인트 링크를 반드시 포함한다.
- `main`에 직접 커밋하지 않는다. 화면/기능 단위로 브랜치 → PR.

## 작성 흐름

```
화면 스펙 작성 → API 엔드포인트 확인 → 구현 → QA
```

1. 새 화면 → `screens/<화면명>/spec.md` ([템플릿](templates/screen.md))
2. 새 API 엔드포인트 → `api/<그룹>.md` ([템플릿](templates/api.md))
3. 새 DB 모델/필드 → `data/<모델명>.md`
4. 주요 설계 결정 → `docs/decisions.md`

## 폴더 구조

| 폴더 | 내용 |
|---|---|
| [screens/](screens/) | 화면별 스펙 (UI · 상태 · API 호출) |
| [api/](api/) | 어드민 API 엔드포인트 명세 |
| [data/](data/) | DB 모델 · 필드 설계 |
| [docs/decisions.md](docs/decisions.md) | 설계 결정 기록 |
| [templates/](templates/) | 문서 템플릿 |

## 현재 구현된 화면

| 화면 | 경로 | 스펙 | 상태 |
|---|---|---|---|
| 대시보드 | `/dashboard` | [spec](screens/dashboard/spec.md) | ✅ 구현 완료 |
| 유저 관리 | `/users` | [spec](screens/users/spec.md) | ✅ 구현 완료 |
| 크루 관리 | `/groups` | [spec](screens/groups/spec.md) | ✅ 구현 완료 |
| 번개 관리 | `/meetups` | [spec](screens/meetups/spec.md) | ✅ 구현 완료 |
| 신고 관리 | `/reports` | [spec](screens/reports/spec.md) | ✅ 구현 완료 |
| 카테고리 | `/categories` | [spec](screens/categories/spec.md) | ✅ 구현 완료 |
| 공지사항 | `/notices` | [spec](screens/notices/spec.md) | ✅ 구현 완료 |
| 푸시 알림 | `/push` | [spec](screens/push/spec.md) | ✅ 구현 완료 |
| 앱 설정 | `/app-settings` | [spec](screens/app-settings/spec.md) | ✅ 구현 완료 |
| 운영 로그 | `/activity-log` | [spec](screens/activity-log/spec.md) | ✅ 구현 완료 |

## 연결 레포

- [crew-admin](https://github.com/900k-develop/crew-admin) — 어드민 프론트엔드 (Vite + React)
- [crew-api](https://github.com/900k-develop/crew-api) — API 서버 (Next.js)
- [crew-docs](https://github.com/900k-develop/crew-docs) — 앱 기획·정책·스펙 문서
