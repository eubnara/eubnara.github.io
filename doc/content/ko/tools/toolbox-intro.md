---
title: "toolbox: 직접 만들어 쓰는 유틸 모음"
date: 2026-05-31T10:00:00+09:00
tags: ["toolbox", "utilities", "linux"]
categories: ["knowledge"]
ShowToc: true
ShowBreadCrumbs: true
---

[toolbox](https://github.com/eubnara/toolbox)는 Linux 데스크탑/노트북 환경을 보강하기 위해 직접 만들어 쓰는 잡다한 유틸 모음 저장소다.
각 도구는 독립된 디렉터리에 자체 README와 설치 스크립트를 갖춘 단위로 구성되어 있다.

현재 등록된 도구는 다음과 같다.

---

## [charger-watts](https://github.com/eubnara/toolbox/tree/main/charger-watts)

USB-C PD 충전기의 실제 협상 와트수를 CLI와 KDE Plasma 6 패널 위젯으로 표시한다.
ThinkPad Embedded Controller의 `HWAT` 필드를 `ec_sys` 모듈로 직접 읽어 와트수를 가져오며, ThinkPad가 아닌 경우 UCSI sysfs로 fallback한다.
Plasma 위젯은 폴링 없이 충전기 연결/해제 이벤트 발생 시에만 EC를 읽어 갱신하는 event-driven 방식이다.

## [pr-keepalive](https://github.com/eubnara/toolbox/tree/main/pr-keepalive)

장기간 방치된 GitHub PR이 stale-bot에 자동으로 닫히지 않도록 주기적으로 짧은 코멘트를 다는 Rust 도구다.
`gh` CLI로 PR 상태를 확인하고, 임계일(기본 80일)을 넘은 PR을 `pending` 상태로 마킹한 후 데스크탑 알림을 띄운다.
사용자가 `--confirm` 플래그로 승인할 때만 실제 코멘트를 포스팅하며, systemd user timer가 부팅 시와 매 24시간마다 자동 실행한다.

## [samsung-clipboard-cleaner](https://github.com/eubnara/toolbox/tree/main/samsung-clipboard-cleaner)

삼성 One UI가 클립보드 기록을 암호화 없이 영구 저장하는 문제를 해결하기 위해 만든 Android(Kotlin) 앱이다.
`ClipboardManager.setPrimaryClip()`을 50회 연속 호출해 더미 클립을 flood하여 기존 기록을 밀어내는 방식으로 동작한다.
`AlarmManager`를 통한 매일 자동 실행(기본 02:00)과 부팅 시 스케줄 재등록을 지원한다.

---

각 도구의 자세한 사용법과 설치 방법은 저장소 내 각 디렉터리의 README를 참고하면 된다.
