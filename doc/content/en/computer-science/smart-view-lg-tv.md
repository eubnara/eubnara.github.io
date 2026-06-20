---
title: "Smart View not working on LG TV — A Chromecast mystery"
date: 2026-06-20T18:00:00+09:00
tags: ["smart-view", "lg-tv", "galaxy-quantum", "chromecast", "troubleshooting"]
categories: ["troubleshooting"]
ShowToc: true
ShowBreadCrumbs: true
---

## The problem

Samsung Galaxy Quantum (SM-A716S, Android 13, One UI 5.1) could not find LG TV 43NU850BENA via Smart View. Galaxy Fold4 could connect after changing the TV network settings (2.4GHz Wi-Fi, IPv6 OFF), but Quantum never could.

## Key observations

| Device | Smart View version | Chromecast support | LG TV found? |
|--------|-------------------|-------------------|-------------|
| Galaxy Quantum (SM-A716S) | 8.2.21.26 | No such option | ❌ |
| Galaxy Fold4 | 8.2.26.40 | ON | ✅ |
| Galaxy Fold4 | 8.2.26.40 | OFF | ❌ |

- Google Home and LG ThinQ apps work perfectly from Quantum to the same TV
- Wi-Fi Direct works normally on Quantum

## Root cause

The LG TV uses **Google Cast (Chromecast built-in)** protocol for Smart View discovery, not Miracast. The Galaxy Quantum's Smart View 8.2.21.26 lacks Google Cast support — the "Chromecast support" toggle present in newer builds is completely absent. Since it's a system app, APK downgrade/upgrade is blocked.

The Fold4 proof: disabling Chromecast support in its Smart View immediately hides the TV from the device list.

## Conclusion

**Smart View on Galaxy Quantum (SM-A716S) does not support Google Cast.** This is a software limitation of that specific Smart View build, not a network or TV issue. Use Google Home app as a fully working alternative — screen mirroring works with no noticeable latency.
