<h1 align="center">⚡ VANTA LOG</h1>

<p align="center">
  <b>Absorb Every Error.</b><br>
  A one-line drop-in web logging console built especially for Android & web developers.<br>
  Instantly visualize console output, network requests, fetch/XHR timings, and JS errors — right inside your webpage.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Under%20Construction-orange?style=for-the-badge&logo=tools" alt="Status">
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20Web-brightgreen?style=for-the-badge&logo=android" alt="Platform">
  <img src="https://img.shields.io/badge/Version-Stable%20Build%20Coming-blueviolet?style=for-the-badge&logo=github" alt="Version">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge&logo=open-source-initiative" alt="License">
  <img src="https://img.shields.io/badge/Made%20for-Developers-00ffaa?style=for-the-badge&logo=codeforces" alt="Made for Developers">
</p>

---

## 🧠 What is VANTA LOG?

VANTA LOG is a **lightweight in-browser debug console overlay** that lets you see all JavaScript logs, network requests, errors, and warnings in one sleek floating UI.  
It’s designed primarily for **Android developers testing locally** using Termux, Pydroid, or mobile browsers — where DevTools are limited or unavailable.

You can literally **paste one script** at the bottom of your HTML file and get a real-time log monitor.  
No build tools. No setup. No dependencies.  
Just pure `<script>` magic.

---

## 🚀 Quick Start (Plug & Play)

Paste this snippet **right before the closing `</body>` tag** of your project’s `index.html`:

```html
https://raw.githack.com/YOCRRZ224/VANTA-LOG/897dde608b00b294ad007fb857ffc8dc0fab1907/Vanta-Log.js
```
Once added, refresh your site — you’ll see the VANTA LOG panel pop up automatically.
 
## Live Demo
https://yocrrz224.github.io/VANTA-LOG
---

## 📱 Why Android Devs Love It

Feature	Description

🧾 Live Log Overlay	Captures console.log, warn, error, debug, and info in real time
⚙️ Network Tracker	Monitors fetch() and XMLHttpRequest calls with response times
🚨 Error Catcher	Captures window.error and unhandledrejection automatically
🌈 Log Filters	Instantly switch between All / Warning / Error / Info / Debug / Resources
💾 Export Logs	Download all captured logs as .json
🧼 Clear Console	One-tap log clear with built-in counter
🎨 Neon Aesthetic UI	Matte glassmorphism overlay with phosphor icons
📱 Mobile-Friendly	Works perfectly in mobile browsers, Termux, and Pydroid
🪄 Hotkey Access	Ctrl + Shift + D toggles visibility
💎 Self-contained	No frameworks, no external dependencies


---

## 🧰 Keyboard Shortcuts

Shortcut	Action

Ctrl + Shift + D	Toggle VANTA LOG visibility
ESC	(Coming Soon) Quick close
⇧ Hover Button	Click to reopen when hidden



---

## 💾 Exported File Format

When you click the Download button, you’ll get a .json file named like:
```
vantalog_2025-11-11_12_45_02.json
```
Each entry looks like:
```
{
  "time": "2025-11-11 12:45:02",
  "type": "error",
  "text": "Uncaught ReferenceError: foo is not defined"
}
```
---

## 🧠 Developer Notes

Works offline, no dependencies.

Designed with Phosphor Icons for clean, scalable visuals.

Compatible with any frontend framework (React, Vue, Svelte, Vanilla JS).

Built for debugging on-the-go in mobile environments.



---

## 🛠️ Upcoming Features (Stable Version)

[ ] Persistent logs using localStorage

[ ] Search bar & live keyword filtering

[ ] Scroll-lock toggle

[ ] Pause / Resume logging

[ ] Collapsible log groups

[ ] Compact/verbose mode

[ ] Multi-theme support

[ ] Remote log streaming (dev dashboards)



---

## ⚠️ Project Status

🚧 UNDER CONSTRUCTION — Stable release coming soon.
The current build is Beta / Experimental, but already functional and stable enough for daily debugging.

> 💡 Want to contribute? Fork it, enhance the UI or performance, and send a PR.




---

## 💚 License

Licensed under the MIT License — you’re free to use, modify, and distribute it.


---

<p align="center">
  <img src="https://img.shields.io/badge/Built%20With-JavaScript-00ffaa?style=for-the-badge&logo=javascript" alt="JS">
  <img src="https://img.shields.io/badge/Frontend-Neon%20Glassmorphism-0a0c12?style=for-the-badge" alt="Theme">
  <img src="https://img.shields.io/badge/Made%20By-YOCRRZ%20%F0%9F%94%A5-00ffaa?style=for-the-badge" alt="Made by YOCRRZ">
</p><p align="center">
  <sub>✨ “Plug it in, light it up, and debug like a legend.” ✨</sub>
</p>
```
---
