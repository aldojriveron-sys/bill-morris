# The 320ms hold can drop the mic during Hey Bill

Hour: 2026-10-06 16:13 EDT. Research only. Do not re-apply the meter release. It is already in `startRec()` on main (`5de3251`, Release the meter before every recognition start). Do not re-apply the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 15:14 note is stale. The update pass already calls `releaseMeter()` before every `rec.start()`. Leave that line alone.

The leftover is the hold timer in `speak()`. `done()` always sets `speaking = false` 320ms later, even when that utterance no longer owns the mic. `sleep()` cancels speech and does not clear `heldUtterance`. `wake()` sets `speaking = true` and aborts the recognizer, but it does not replace `heldUtterance` until `speak()` runs. If Hey Bill lands inside that 320ms window, the old timer clears `speaking` while `wake()` is still in `openMeter()`, and a non-wake final can reach `handle()`. Same class of gap the 14:13 note closed, one timer later.

Null the held utterance when wake takes the mic, and only clear `speaking` if this utterance is still held. The 320ms tail stays for the line that still owns the mic. Arm, sleep, and the hidden-tab restart stay the same. No new account, no model, no longer mute. Free. Safe.

Phrase boost 8 is inside the 0.0–10.0 range. No phrase diff. MDN still warns that 9 or 10 can make other phrases look like Hey Bill. Leave the bias at 8.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`). One tap still arms Chrome.

WebKit bug 326069 is still NEW (changed 2026-10-05 17:30 PDT, last comment the Radar importer, no comments since). The bug notes say a new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This timer gate does not claim to fix iOS 27.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, two hunks.

In `wake()`, the line after `speaking = true`:

Add

`heldUtterance = null;`

In `speak()`, the 320ms timer, replace

```
setTimeout(() => {
  speaking = false;
  if (heldUtterance === u) heldUtterance = null;
  resolve();
}, 320);
```

with

```
setTimeout(() => {
  if (heldUtterance === u) {
    speaking = false;
    heldUtterance = null;
  }
  resolve();
}, 320);
```

Do not change `startRec()`, `releaseMeter()`, `openMeter()`, `handle()`, the sleep lastBill assign, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or `sleep()`. Do not add a local model.
