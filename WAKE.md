# Phone Chrome has no Hey Bill phrase bias

Hour: 2026-10-06 01:13 EDT. Research only. The 01:07 update already applied phrase bias. Do not re-apply it. Do not set processLocally this hour.

## What I learned

Desktop Chrome 142+ has `SpeechRecognitionPhrase`. The live desk browser here exposes the constructor. Can I use still lists Chrome for Android as not supported (checked at 154) and Safari on iOS as not supported through 27.2. The boost of 8 on main is in the legal 0.0–10.0 range, but that line never runs on the phone. Phone wake is still the regex in `hearWake`: optional hey/ok/okay, then bill. Chrome often hides the right phrase in a later alternative and returns only alternative 0 unless `maxAlternatives` is raised. The desk still reads only `[0]`.

Live page at 390×844 does not overflow. Arm stays a 52px tap, the hint stays on screen, the open line is still one sentence (`I'm here.`). Sleep on main already returns to the wake word. No layout diff.

Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## Still blocking

Netlify free credits still block publish. Live https://bill-morris.netlify.app still says “say Bill” and can stay behind main. WebKit bug 326069 is still open: iOS may hear only the first session in a tab. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `startRec`:

After `rec.continuous = true;` add `rec.maxAlternatives = 3;`

In `rec.onresult`, before reading alternative 0, if `!awake` scan that result's alternatives and call `wake()` on the first `hearWake` hit. Keep using alternative 0 for the awake command path. Do not change `hearWake`, `speak()`, the open line, the phone rule, sleep, or the phrase-bias block.
