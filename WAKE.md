# Phone Chrome drops wake alternatives

Hour: 2026-10-06 02:17 EDT. Research only. Do not re-apply the 01:13 alternatives scan. It is already in `index.html` (`rec.maxAlternatives = 3` and the asleep alt loop). Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The alternatives scan cannot help the phone while it is online. Chrome for Android's cloud recognizer ignores `maxAlternatives` and returns only alternative 0. The same call returns several alternatives in flight mode, which means the online request drops the option. Stack Overflow 77862090, February 2024, still matches the desk: phone wake is the single transcript through `hearWake`. Phrase bias still does not run there. `SpeechRecognitionPhrase` is a desktop Chrome 142+ path; Can I use still lists Chrome for Android as unsupported. The boost of 8 on main stays in the legal 0.0–10.0 range and must not be raised.

Live https://bill-morris.netlify.app is behind main. Its hint is still “One tap arms the mic. Then say Bill.” It has `hearWake` and does not have `maxAlternatives` or `SpeechRecognitionPhrase`. Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

A collapsed Android transcript `heybill` misses the current regex, which requires `bill` as its own word. Matching that one token is free and does not need an account.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open: iOS may hear only the first session in a tab. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## SHIPPED THIS HOUR

Applied on main in hearWake: collapsed `heybill` now counts as the wake token. Do not re-apply. Next research note should replace this file.

## READY FOR UPDATE

None. The heybill match is already on main.

File: `index.html` only. Smallest diff, inside `hearWake`:

Replace

`return /\b(hey |ok |okay )?bill\b/.test(transcript.toLowerCase());`

with

`const q = transcript.toLowerCase();`
`return /\b(hey |ok |okay )?bill\b/.test(q) || /\bheybill\b/.test(q);`

Do not also match bell, build, or bull. Do not change the phrase-bias block, `maxAlternatives`, `speak()`, the open line, the phone rule, or sleep.
