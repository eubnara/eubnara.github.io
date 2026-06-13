# 개인정보처리방침 / Privacy Policy

**ClipFlood** (이하 "앱")는 삼성 One UI 클립보드 기록을 정리하는 도구입니다.

**ClipFlood** ("the App") is a utility that clears Samsung One UI clipboard history.

---

## 1. 수집하는 데이터 / Data Collection

**이 앱은 어떤 개인정보도 수집하지 않습니다.**

**This app does not collect any personal data.**

- 인터넷 권한 없음 — 네트워크 통신 안 함
- No internet permission — no network communication
- 사용자 계정, 위치, 연락처, 카메라, 마이크 등에 접근하지 않음
- No access to accounts, location, contacts, camera, microphone, etc.
- 클립보드 데이터를 **읽지 않음** — `ClipboardManager.setPrimaryClip()`만 호출하여 더미 데이터를 기록
- Does **not read** clipboard data — only calls `ClipboardManager.setPrimaryClip()` to write dummy clips

## 2. 저장하는 데이터 / Stored Data

앱 설정(스케줄 시간, 플러드 개수)만 기기 내 `SharedPreferences`에 저장됩니다.
Only app preferences (schedule time, flood count) are stored locally via `SharedPreferences`.

이 데이터는 앱 삭제 시 함께 삭제됩니다.
This data is deleted when the app is uninstalled.

## 3. 제3자 공유 / Third-Party Sharing

어떤 데이터도 제3자와 공유되지 않습니다.
No data is shared with any third party.

## 4. 권한 / Permissions

| Permission | Purpose |
|---|---|
| `RECEIVE_BOOT_COMPLETED` | Re-schedule after reboot |
| `POST_NOTIFICATIONS` | Foreground service notification |
| `FOREGROUND_SERVICE` | Background flood execution |
| `SCHEDULE_EXACT_ALARM` | Daily scheduled execution |
| `USE_EXACT_ALARM` | Exact alarm (Android 14+) |

이 권한들은 오직 앱의 핵심 기능(클립보드 정리 및 스케줄)을 위해서만 사용됩니다.
These permissions are used solely for the app's core functionality.

## 5. 문의 / Contact

**이름 / Name:** EUB works
**이메일 / Email:** eubworks@gmail.com

---

*최종 업데이트 / Last updated: 2026-06-13*
