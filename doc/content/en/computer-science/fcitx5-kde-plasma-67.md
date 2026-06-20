---
title: "Fcitx5 modifier keys broken after KDE Plasma 6.7 upgrade"
date: 2026-06-20T18:30:00+09:00
tags: ["fcitx5", "kde", "kde-plasma", "kwin", "wayland", "korean-input", "troubleshooting"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

## Symptom

After `pkcon update` on KDE Neon (Plasma 6.7, KWin 6.7.0):

- Korean input works
- Shift/Ctrl modifier keys break in some apps
  - `Shift + -` does not produce `_` (underscore)
  - `Ctrl + a` acts as plain `a`
  - But `Shift + ㅂ → ㅃ` (double consonant) works fine
- Disabling fcitx5 fixes modifier keys → input method is the culprit

## Root cause: version skew

| Component | Version | Status |
|-----------|---------|--------|
| apt fcitx5 | 5.1.7 | Too old |
| KWin | 6.7.0 | Too new |

Fcitx processes keys in two ways:

| Type | Example | Path |
|------|---------|------|
| Consume (input method handles) | 한글, Shift+ㅂ→ㅃ | fcitx eats it → fine |
| Forward (fcitx ignores) | `_`, `Ctrl+a` | fcitx returns to KWin → **broken** |

When forwarding, fcitx must pass modifier (Shift/Ctrl) state back to the compositor. **Fcitx 5.1.7 on KWin 6.7.0 drops modifiers during forward.** This was fixed in **fcitx 5.1.13**.

### Why only some apps broke

Fcitx communicates with apps via 3 paths:

| Path | Mechanism | Apps | Affected? |
|------|-----------|------|-----------|
| ① D-Bus IM module | Native GTK/Qt load frontend shared lib | konsole (Qt) | ✅ No |
| ② Wayland input-method | Compositor relays (no frontend lib) | AppImage, Electron | ❌ Yes |
| ③ XIM | XWayland | X11 apps | ✅ No |

- **konsole**: Path ① (D-Bus directly to fcitx daemon, bypassing KWin) → **fine**
- **ghostty** (AppImage): No bundled fcitx frontend → falls to Path ② → **KWin bug hits directly** → broken

## The fix: flatpak fcitx5 (5.1.19)

Since apt's fcitx5 (5.1.7) cannot be upgraded on KDE Neon 24.04, replace only the daemon with flatpak.

### 1. Install flatpak fcitx5 + Hangul addon

```
flatpak install flathub org.fcitx.Fcitx5
flatpak install flathub org.fcitx.Fcitx5.Addon.Hangul
```

Hangul is a **separate extension** — not included in the main package.

### 2. Copy existing config

```
mkdir -p ~/.var/app/org.fcitx.Fcitx5/config/fcitx5
cp -rf ~/.config/fcitx5/* ~/.var/app/org.fcitx.Fcitx5/config/fcitx5/
```

### 3. Stop host fcitx, register flatpak autostart

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

### 4. Environment variables (host → flatpak daemon)

`~/.config/environment.d/fcitx5.conf`:

```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

### 5. Re-login

## Why keep host fcitx5-frontend packages

The flatpak fcitx5 provides the **daemon (engine)** only. Native host apps need **host IM modules** (`/usr/lib/.../fcitx5-frontend-*.so`) loaded into their process to connect via D-Bus.

```
[ flatpak fcitx5 5.1.19 daemon ]
  ▲                   ▲                    ▲
  │ D-Bus             │ D-Bus              │ Wayland
[host frontend .so]  [host frontend .so]  [AppImage self GTK]
  │                   │                    │
konsole              native apps          ghostty
```

Required apt packages (flatpak cannot provide these):

```
sudo apt install fcitx5-frontend-gtk3 fcitx5-frontend-gtk4 \
                 fcitx5-frontend-qt5 fcitx5-frontend-qt6
```

**Watch out**: these may be marked `auto-removable`. Prevent accidental deletion:

```
sudo apt-mark manual fcitx5-frontend-gtk3 fcitx5-frontend-gtk4 \
                     fcitx5-frontend-qt5 fcitx5-frontend-qt6
```

## When can we switch back?

Once apt's fcitx5 reaches **5.1.13+**, revert:

1. Remove flatpak autostart
2. Restore host fcitx5 autostart
3. Remove flatpak daemon

## Summary

| Component | Source | Note |
|-----------|--------|------|
| fcitx5 daemon | flatpak (5.1.19) | Newer than apt |
| Hangul addon | flatpak | Separate extension |
| fcitx5-frontend-* | apt | Required for native apps |
| Env vars / autostart / config | manual | Not in official docs |
