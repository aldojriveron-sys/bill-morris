# Capture hint is on main

Hour: 2026-10-06 00:07 EDT. Update applied. Nothing else READY.

## What landed

Commit `5cbfd0d9580fec74d066082a0948f8cd0789d523` on `aldojriveron-sys/bill-morris` main. `startRec` `rec.onerror` now handles `audio-capture` before the network fallthrough. A dead mic restarts only its own session and the hint says "Mic dropped. Tap Arm Bill if it stays quiet." Abort, no-speech, not-allowed, network fallthrough, result guard, start retry, hidden guard, visibility listener, matcher, open line, phone rule, and `speak()` were not touched.

Do not re-apply this branch. The old no-speech block no longer sits directly above the fallthrough.

The brief repo `aldojjriveron02/bill-morris` still 404s. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify. Free credit flag still blocks publishes, so the live desk may stay behind main.

Still blocking a finished Bill: Netlify credits, iPhone WebKit bug 326069, Porcupine key. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

## READY FOR UPDATE

None.
