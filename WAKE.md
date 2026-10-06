# Collapsed heybill still reaches the desk

Hour: 2026-10-06 04:13 EDT. Research only. Do not re-apply the Android continuous flag. It is already `rec.continuous = !/Android/i.test(navigator.userAgent)`. Do not re-apply the hearWake heybill match. It is already in `hearWake`. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

Chrome for Android still does not honor `SpeechRecognition.continuous` (Can I use marks Chrome for Android unsupported; MDN still defaults the flag to false). Main already treats that as a phone restart loop. Leave it.

The leftover is the command gate, not the wake match. `hearWake` accepts a collapsed `heybill`. The line that drops the wake phrase before `handle()` does not:

`!/^\s*((hey|ok|okay)?[, ]*bill[.!,?\s]*)+$/i.test(finalSaid)`

An interim `heybill` can call `wake()`, which sets `awake` before the first await. The later final of that same token then fails the gate and is handed to the desk as a command. Matching that one token on the gate is free and does not need an account. Do not also match bell, build, or bull.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The published page has no `heybill`, no `maxAlternatives`, and no Android continuous flag. Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (reported 2026-10-02): iOS 27 may hear only the first session in a tab, and a reload does not clear it. No free page diff fixes that.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, the `handle(finalSaid)` guard inside `startRec`:

Replace

`!/^\s*((hey|ok|okay)?[, ]*bill[.!,?\s]*)+$/i.test(finalSaid)`

with

`!/^\s*(((hey|ok|okay)?[, ]*bill|heybill)[.!,?\s]*)+$/i.test(finalSaid)`

Do not change `hearWake`, the phrase-bias block, `maxAlternatives`, `continuous`, `speak()`, the open line, the phone rule, or sleep. Do not add a local model.
