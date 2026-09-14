# ⚔️ Battle Scars — AI Agent Cluster Pitfall Manual (Open Edition)

> Real bugs from a multi-agent cluster (1 manager + 4 agents on one Windows machine), updated weekly.
> Format: **Symptom → Root Cause → Fix → Repro**. No theory, all scars.

**⏳ This manual is actively updated — new cases every week. Watch the repo to catch updates.**

---

## Case 01 — PowerShell 5.1 writes UTF-8 files WITH BOM, downstream JSON parsers choke

**Symptom**: A JSON task file written by PowerShell gets rejected by a Node.js gateway with a cryptic parse error. The JSON looks perfectly valid when you open it.

**Root cause**: PowerShell 5.1's `-Encoding UTF8` writes UTF-8 **with BOM** (EF BB BF). Many strict JSON parsers (and HTTP headers built from file contents) don't strip it.

**Fix**: Never write machine-consumed files with PS 5.1. Use Node.js:

```js
// node writes UTF-8 without BOM, always
import { writeFileSync } from 'node:fs';
writeFileSync('task.json', JSON.stringify(payload), 'utf8');
```

**Bonus trap**: If a BOM-poisoned file already exists, `string.trim()` will NOT save you — `\uFEFF` is not whitespace. Strip explicitly: `s.replace(/^\uFEFF/, '')`.

---

## Case 02 — CDP debug port dies after app restart (silent agent blackout)

**Symptom**: Your automation that types into an Electron app (WeChat/Doubao/Coze desktop etc.) suddenly fails with `ECONNREFUSED 127.0.0.1:<port>`.

**Root cause**: The app was restarted (by user, updater, or crash) WITHOUT `--remote-debugging-port`. The port only exists if you pass it at launch. Process alive ≠ port alive.

**Fix**: Check both layers before dispatching:

```powershell
$alive = Get-Process DuMate -ErrorAction SilentlyContinue
$port  = Test-NetConnection 127.0.0.1 -Port 9555 -InformationLevel Quiet -WarningAction SilentlyContinue
if ($alive -and -not $port) {
  Get-Process DuMate | Stop-Process -Force
  Start-Process "D:\apps\DuMate.exe" -ArgumentList "--remote-debugging-port=9555"
}
```

**Lesson**: Health check = process AND port. Write a preflight into your dispatcher.

---

## Case 03 — Screen capture grabs the WRONG window (CopyFromScreen + no foreground)

**Symptom**: You capture "window A" but the screenshot shows the desktop or another app entirely. You send it to a vision model and it confidently describes the wrong thing. Embarrassment ensues.

**Root cause**: `CopyFromScreen` captures whatever is actually on screen. If your target window isn't foreground, you get whatever is on top.

**Fix**: Use `PrintWindow` with `PW_RENDERFULLCONTENT` (flag = 2). It captures the window's own surface even if occluded:

```powershell
Add-Type -AssemblyName System.Drawing
# P/Invoke PrintWindow(hwnd, hdc, 2) — flag 2 = PW_RENDERFULLCONTENT
# Works for hardware-accelerated (Chromium/Electron) windows, no focus needed
```

---

## Case 04 — Electron MainWindowHandle is a 56x56 tray icon

**Symptom**: `Process.MainWindowHandle` gives you a tiny window; screenshots come out 56x56 px.

**Root cause**: Some Electron apps report their tray icon window as MainWindowHandle when the main window is minimized/backgrounded.

**Fix**: Enumerate ALL top-level windows for the PID, filter visible ones > 300x300, pick the largest by area.

---

## Case 05 — WeChat media bridge: rapid multi-file sends trigger `ret=-2 session prepare failed`

**Symptom**: Sending 4 files back-to-back through the WeChat outbox pattern fails after the 1st-2nd file with `ret=-2`.

**Root cause**: The WeChat session goes to sleep between rapid automated sends (the bridge's session needs a wake).

**Fix**: Send files ONE at a time with a delay. If the bridge is stuck, have a human send any message to the chat — it wakes the session. Real errors live in `outbox-errors.log`, not the `.err.json`.

---

*More cases every week — watch/star to collect them all.*
*Full extended manual (100+ cases, repro scripts, monitoring recipes): see README contact section.*
