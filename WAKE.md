# Desktop still opens a second mic after Hey Bill

Hour: 2026-10-06 15:14 EDT. Research only. Do not re-apply the wake-gap hold. It is already in `wake()` on main (`e92b72b7`, Hold the mic before wake yields to the open line). Do not re-apply the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the 320ms hold, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 14:13 note is stale. The update pass already set `speaking = true` and aborted the recognizer before `await openMeter()`. Leave `wake()` alone.

The leftover is the second capture on desktop. `phoneMic()` already skips `openMeter()` and `startRec()` already calls `releaseMeter()` before `rec.start()` on a phone, including an iPad that reports a Macintosh UA. That was so SpeechRecognition stayed the only capture. Desktop never takes that path. After Hey Bill, `wake()` still calls `openMeter()`, which holds a `getUserMedia` stream, then `startRec()` starts Chrome recognition while those tracks are live. Chromium 391899145 is the same class of failure: a second audio capture aborts the first. The phone guard already chose the recognizer over the bars. Desktop should do the same, or the session after the open line can sit on silence or `audio-capture`.

Release the meter on every start, not only on a phone. `releaseMeter()` is a no-op when no stream is open, so arm, sleep, and the hidden-tab restart stay the same. Bars can move during the open line, then go dark while Chrome is listening. That is the point. No new account, no model, no longer mute. Free. Safe.

Phrase boost 8 is inside the 0.0–10.0 range. No phrase diff. MDN still warns that 9 or 10 can make other phrases look like Hey Bill. Leave the bias at 8.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (changed 2026-10-05 17:30 PDT, last comment the Radar importer, no comments since). The bug notes say a new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This meter release does not claim to fix iOS 27.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `startRec()`, the line before `rec.start()`:

Replace

`if (phoneMic()) releaseMeter();`

with

`releaseMeter();`

Do not change `wake()`, `handle()`, the sleep lastBill assign, `speak()`, the 320ms timer, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or `sleep()`. Do not add a local model.
