# Device 모델

유저의 기기 정보 및 FCM 푸시 토큰 관리.

- 결정 배경: [docs/decisions.md](../docs/decisions.md)
- 관련 API: [api/devices.md](../api/devices.md)

## Prisma 스키마

```prisma
model Device {
  id           String   @id @default(cuid())
  userId       String
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  os           String   // "ios" | "android"
  osVersion    String?  // "17.4" | "14"
  appVersion   String?  // "1.0.0"
  model        String?  // "iPhone 15 Pro" | "samsung SM-S921N"
  fcmToken     String?  @unique
  lastActiveAt DateTime @default(now())
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt

  @@index([userId])
}
```

## 수집 방법

앱 실행 또는 로그인 직후 `POST /api/devices` 호출.

### iOS (Swift)

```swift
import UIKit
import FirebaseMessaging

func registerDevice() async {
    guard let fcmToken = try? await Messaging.messaging().token() else { return }
    let payload: [String: Any] = [
        "os": "ios",
        "osVersion": UIDevice.current.systemVersion,
        "appVersion": Bundle.main.infoDictionary?["CFBundleShortVersionString"] as? String ?? "",
        "model": UIDevice.current.model,
        "fcmToken": fcmToken
    ]
    try? await api.post("/devices", body: payload)
}
```

### Android (Kotlin)

```kotlin
FirebaseMessaging.getInstance().token.addOnSuccessListener { fcmToken ->
    val payload = mapOf(
        "os" to "android",
        "osVersion" to Build.VERSION.RELEASE,
        "appVersion" to BuildConfig.VERSION_NAME,
        "model" to "${Build.MANUFACTURER} ${Build.MODEL}",
        "fcmToken" to fcmToken
    )
    viewModelScope.launch { api.post("/devices", payload) }
}
```

## API — POST /api/devices

인증된 유저의 기기 등록 또는 갱신.

```
fcmToken unique → upsert
  - 같은 토큰이면 lastActiveAt, appVersion, osVersion 갱신
  - 새 토큰이면 새 Device 생성
```

**요청**

```json
{
  "os": "ios",
  "osVersion": "17.4",
  "appVersion": "1.0.0",
  "model": "iPhone",
  "fcmToken": "abc123..."
}
```

**응답**

```json
{ "ok": true }
```

## 어드민 — 유저 상세 패널

`GET /api/admin/users/:id` 응답에 `devices: Device[]` 포함.

표시 항목: OS · 모델 · 앱 버전 · FCM 토큰 앞 8자 · 마지막 접속일

## 마이그레이션 계획

1. `Device` 테이블 추가 → `npx prisma migrate dev --name add-device`
2. `users.pushToken` → `Device` 테이블로 데이터 이관 (스크립트)
3. `users.pushToken` deprecated — 푸시 발송 로직을 `Device.fcmToken` 기준으로 전환
4. `users.pushToken` 컬럼 제거 (별도 마이그레이션)

## 현황

- [ ] Prisma 스키마 추가
- [ ] `POST /api/devices` 엔드포인트
- [ ] iOS 앱 — 로그인 후 호출 추가
- [ ] Android 앱 — 로그인 후 호출 추가
- [ ] 어드민 유저 패널 — 기기 목록 표시
- [ ] `users.pushToken` → `Device` 이관
