---
title: "LG TV Smart View 연결 안 됨 — 크롬캐스트를 찾아서"
date: 2026-06-20T18:00:00+09:00
tags: ["smart-view", "lg-tv", "galaxy-quantum", "chromecast", "troubleshooting"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

## 문제

갤럭시 퀀텀(SM-A716S, Android 13, One UI 5.1)의 Smart View로 LG TV 43NU850BENA를 찾을 수 없었다. 갤럭시 폴드4는 TV 네트워크 설정(2.4GHz Wi-Fi, IPv6 OFF) 변경 후 연결이 되었지만, 퀀텀은 끝까지 안 되었다.

## 관찰 결과

| 기기 | Smart View 버전 | Chromecast 지원 | LG TV 발견? |
|------|----------------|----------------|------------|
| 갤럭시 퀀텀 (SM-A716S) | 8.2.21.26 | 옵션 자체 없음 | ❌ |
| 갤럭시 폴드4 | 8.2.26.40 | ON | ✅ |
| 갤럭시 폴드4 | 8.2.26.40 | OFF | ❌ |

- Google Home과 LG ThinQ 앱은 퀀텀에서 동일 TV로 정상 연결됨
- Wi-Fi Direct는 퀀텀에서 정상 동작

## 원인

해당 LG TV는 Smart View 검색에 **Google Cast(Chromecast built-in)** 프로토콜을 사용한다. 갤럭시 퀀텀의 Smart View 8.2.21.26은 Google Cast를 지원하지 않는 빌드이며, "Chromecast 지원" 토글 자체가 없다. 시스템 앱이므로 APK 교체도 불가능하다.

폴드4의 실험이 결정적이었다: Chromecast 지원을 끄자 TV가 목록에서 즉시 사라졌다.

## 결론

**갤럭시 퀀텀(SM-A716S)의 Smart View는 Google Cast를 지원하지 않는다.** 네트워크나 TV 문제가 아니라 해당 Smart View 빌드의 소프트웨어적 한계다. Google Home 앱으로 화면 미러링이 정상 동작하므로 이를 대안으로 사용하면 된다.
