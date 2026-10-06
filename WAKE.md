# Phone Chrome ignores continuous

Hour: 2026-10-06 03:15 EDT. Research only. Do not re-apply the 02:17 heybill match. It is already in `hearWake`. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

Chrome for Android can set `SpeechRecognition.continuous`, but the flag has no effect. MDN marks the property unsupported there: the session still ends after one result. The desk sets `rec.continuous = true` for every browser, then restarts from `onend` at 250ms. That restart is already the phone wake loop. A held-open mic is a desktop behavior, not a phone one.

Setting continuous false on Android only is free and does not need an account. On Chrome Android it matches the ignored flag. On other Android browsers, continuous true is the unstable path (no-speech and busy loops). Leave iPhone on true: Safari iOS does honor continuous, and WebKit 326069 already limits a tab to one good session. Do not call `SpeechRecognition.available({ processLocally: true })` on the phone. A May 2026 Chrome Android trace still returns `unavailable` because the on-device model is not shipped. A call there would fail the arm.

Live https://bill-morris.netlify.app is still behind main. Hint is still "One tap arms the mic. Then say Bill." The published page has no `heybill` and no `maxAlternatives`. Phone layout on main was already clear at 390×844. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open (reported 2026-10-02, status NEW): iOS 27 may hear only the first session in a tab, and a reload does not clear it. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `startRec`:

Replace

`rec.continuous = true;`

with

`rec.continuous = !/Android/i.test(navigator.userAgent);`

Do not change `hearWake`, the phrase-bias block, `maxAlternatives`, `speak()`, the open line, the phone rule, or sleep. Do not add a local model.
