# App Settings 스펙

- 경로: `/app-settings`
- 구현 파일: `src/ui/screens/AppSettingsScreen.tsx`
- 관련 API: [api/settings.md](../../api/settings.md)

## 1. 목적

서비스 운영 설정 관리. 점검 모드 on/off, 최소/강제 업데이트 버전 설정.

## 2. UI 구성

| 영역 | 설명 |
|---|---|
| 점검 모드 카드 | 토글 스위치 + 상태 뱃지 (정상 운영 / 점검 중) |
| 점검 메시지 | 점검 모드 활성 시에만 노출되는 textarea |
| 버전 관리 카드 | iOS/Android 최소 버전·강제 업데이트 버전 4개 input |
| 저장 버튼 | 전체 설정 일괄 PATCH + "✓ 저장됨" 3초 표시 |

### 설정 필드

| 필드 | 타입 | 설명 |
|---|---|---|
| `maintenanceMode` | boolean | 점검 모드 활성 여부 |
| `maintenanceMessage` | string | 앱에 표시할 점검 메시지 |
| `iosMinVersion` | string | iOS 최소 지원 버전 (미만 시 업데이트 권고) |
| `androidMinVersion` | string | Android 최소 지원 버전 |
| `iosForceVersion` | string | iOS 강제 업데이트 기준 버전 (미만 시 강제) |
| `androidForceVersion` | string | Android 강제 업데이트 기준 버전 |

## 3. API 호출

| 액션 | 메서드 | 엔드포인트 | 비고 |
|---|---|---|---|
| 설정 조회 | GET | `/api/admin/settings` | 6개 필드 반환 |
| 설정 변경 | PATCH | `/api/admin/settings` | 전체 필드 일괄 전송 |

## 4. 완료 기준

- [x] 설정 조회 및 렌더
- [x] 점검 모드 토글
- [x] 점검 메시지 입력 (점검 모드 활성 시 노출)
- [x] 버전 관리 (iOS/Android 최소·강제 4개 필드)
- [x] 설정 저장 (PATCH)
- [x] 에러 상태 처리
