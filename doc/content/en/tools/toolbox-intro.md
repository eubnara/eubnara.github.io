---
title: "toolbox: A collection of DIY utilities"
date: 2026-05-31T10:00:00+09:00
tags: ["toolbox", "utilities", "linux"]
categories: ["knowledge"]
ShowToc: true
ShowBreadCrumbs: true
---

[toolbox](https://github.com/eubnara/toolbox) is a collection of miscellaneous utilities I built to enhance my Linux desktop/laptop environment.
Each tool lives in its own directory with its own README and install script.

Currently available tools:

---

## [charger-watts](https://github.com/eubnara/toolbox/tree/main/charger-watts)

Displays the negotiated wattage of a connected USB-C PD charger via CLI and a KDE Plasma 6 panel widget.
Reads the `HWAT` field directly from the ThinkPad Embedded Controller via the `ec_sys` module, with a fallback to UCSI sysfs for non-ThinkPad machines.
The Plasma widget is event-driven — it reads the EC only when a charger is plugged or unplugged, with no polling.

## [pr-keepalive](https://github.com/eubnara/toolbox/tree/main/pr-keepalive)

A Rust tool that periodically posts short comments on long-idle GitHub PRs to prevent stale-bot from auto-closing them.
Uses the `gh` CLI to check PR status, marks PRs older than a threshold (default 80 days) as `pending`, and shows a desktop notification.
Only posts the comment when the user approves with the `--confirm` flag. A systemd user timer runs it at boot and every 24 hours.

## [samsung-clipboard-cleaner](https://github.com/eubnara/toolbox/tree/main/samsung-clipboard-cleaner)

An Android (Kotlin) app that automatically clears Samsung One UI clipboard history, addressing a security concern where clipboard data is stored permanently without encryption.
Works by calling `ClipboardManager.setPrimaryClip()` 50 times in succession to flood the clipboard with dummy entries, pushing out the old history.
Supports daily auto-run via `AlarmManager` (default 02:00) and reschedules on boot.

---

See each tool's README for detailed usage and installation instructions.
