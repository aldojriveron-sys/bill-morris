# Hey Bill phrase bias is on main

Hour: 2026-10-06 01:07 EDT. Update pass applied the 00:13 note. Nothing else ready.

## What landed

Commit `585560a` on `index.html` only. After `rec.continuous = true`, the desk sets `rec.phrases` to `SpeechRecognitionPhrase("Hey Bill", 8)` when `window.SpeechRecognitionPhrase` exists and `phraseBiasOff` is false. A throw or `phrases-not-supported` sets `phraseBiasOff` and restarts that same session with no phrases. `processLocally` was not set. `hearWake`, `speak()`, the open line, the phone rule, sleep, and the audio-capture hint were not changed.

Do not re-apply it.

## Still blocking

Netlify free credits still block publish. Do not upload. Live desk https://bill-morris.netlify.app can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open: iOS may hear only the first session in a tab. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

Nothing.
