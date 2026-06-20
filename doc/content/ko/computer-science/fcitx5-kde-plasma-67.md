---
title: "KDE Plasma 6.7 업데이트 후 fcitx5 조합키 깨짐 해결"
date: 2026-06-20T18:30:00+09:00
tags: ["fcitx5", "kde", "kde-plasma", "kwin", "wayland", "korean-input", "troubleshooting"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

## 증상

KDE Neon에서 `pkcon update` 후 Plasma 6.7 (KWin 6.7.0)으로 올라가면서:

- 한글 입력 자체는 정상
- 일부 앱에서 Shift/Ctrl 조합키가 동작하지 않음
  - `Shift + -` → `_` 안 됨
  - `Ctrl + a` → 그냥 `a`로 입력
  - 하지만 `Shift + ㅂ → ㅃ` (쌍자음)은 정상
- fcitx5를 완전히 끄면 조합키 정상 → 입력기 문제 확인

## 원인: 버전 불일치 (version skew)

| 구성 요소 | 버전 | 상태 |
|-----------|------|------|
| apt fcitx5 | 5.1.7 | 너무 구버전 |
| KWin | 6.7.0 | 너무 신버전 |

fcitx5는 키 입력을 두 가지 방식으로 처리한다:

| 방식 | 예 | 경로 |
|------|-----|------|
| 소비 (consume) | 한글, Shift+ㅂ→ㅃ | fcitx가 처리 → 정상 |
| 전달 (forward) | `_`, `Ctrl+a` 등 | fcitx가 KWin에 반환 → **깨짐** |

forward 시 fcitx는 modifier(Shift/Ctrl) 상태를 함께 전달해야 하는데, **5.1.7 + KWin 6.7.0 조합에서 modifier가 유실되었다.** 이 문제는 **fcitx 5.1.13부터 수정**되었다.

### 앱마다 증상이 달랐던 이유

fcitx는 앱과 3가지 경로로 통신한다:

| 경로 | 방식 | 해당 앱 | 영향 |
|------|------|---------|------|
| ① D-Bus IM 모듈 | 네이티브 GTK/Qt가 frontend 공유 라이브러리 로드 | konsole (Qt) | ✅ 정상 |
| ② Wayland input-method | 컴포지터(KWin)가 중계 | AppImage, Electron | ❌ 깨짐 |
| ③ XIM | XWayland | X11 앱 | ✅ 정상 |

- **konsole**: 경로 ① (D-Bus로 fcitx 데몬에 직접 접속, KWin 우회) → **정상**
- **ghostty** (AppImage): 자체 GTK에 fcitx 모듈 없음 → 경로 ②로 폴백 → **KWin 버그 직격** → 조합키 깨짐

## 해결 방법

apt fcitx5(5.1.7)를 업그레이드할 수 없으므로, **flatpak으로 최신 fcitx5(5.1.19)**를 받아 데몬만 교체.

### 1. flatpak fcitx5 + 한글 엔진 설치

```
flatpak install flathub org.fcitx.Fcitx5
flatpak install flathub org.fcitx.Fcitx5.Addon.Hangul
```

한글 엔진은 **별도 extension** — 본체에 포함되지 않음.

### 2. 기존 설정 복사

```
mkdir -p ~/.var/app/org.fcitx.Fcitx5/config/fcitx5
cp -rf ~/.config/fcitx5/* ~/.var/app/org.fcitx.Fcitx5/config/fcitx5/
```

### 3. host fcitx 중단, flatpak 자동시작 등록

```
pkill fcitx5
rm -f ~/.config/autostart/org.fcitx.Fcitx5.desktop
```

```
cat > ~/.config/autostart/fcitx5-flatpak.desktop <<'EOF'
[Desktop Entry]
Type=Application
Name=Fcitx5 (Flatpak)
Exec=flatpak run org.fcitx.Fcitx5
EOF
```

### 4. 환경변수 설정

`~/.config/environment.d/fcitx5.conf`:

```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

### 5. 재로그인

## host fcitx5-frontend 패키지를 유지해야 하는 이유

flatpak fcitx5는 **데몬(엔진)**만 제공한다. 네이티브 host 앱(konsole 등)은 자신의 프로세스 안에 **host용 IM 모듈**(`/usr/lib/.../fcitx5-frontend-*.so`)을 로드해야 D-Bus로 데몬에 접속할 수 있다.

```
[ flatpak fcitx5 5.1.19 데몬 ]
  ▲                   ▲                    ▲
  │ D-Bus             │ D-Bus              │ Wayland
[host frontend .so]  [host frontend .so]  [AppImage 자체 GTK]
  │                   │                    │
konsole              네이티브 앱           ghostty
```

apt로 따로 설치해야 하는 패키지 (flatpak이 제공 불가):

```
sudo apt install fcitx5-frontend-gtk3 fcitx5-frontend-gtk4 \
                 fcitx5-frontend-qt5 fcitx5-frontend-qt6
```

**주의**: 이 패키지들은 `auto-removable`로 표시될 수 있다. 실수로 지워지는 것 방지:

```
sudo apt-mark manual fcitx5-frontend-gtk3 fcitx5-frontend-gtk4 \
                     fcitx5-frontend-qt5 fcitx5-frontend-qt6
```

## 언제 apt로 돌아갈 수 있나

apt의 fcitx5가 **5.1.13+**로 올라오면 복귀 가능:

1. flatpak 자동시작 제거
2. host fcitx5 자동시작 복원
3. flatpak 데몬 제거

## 요약

| 구성 요소 | 설치 방법 | 비고 |
|-----------|-----------|------|
| fcitx5 데몬 | flatpak (5.1.19) | apt보다 최신 |
| 한글 엔진 | flatpak | 별도 extension |
| fcitx5-frontend-* | apt | 네이티브 앱에 필수 |
| 환경변수 / 자동시작 / 설정 | 수동 | 공식 문서에 없음 |
