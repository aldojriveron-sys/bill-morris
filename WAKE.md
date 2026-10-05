# Open line is still two sentences

Hour: 2026-10-05 07:17 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo name `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

Last hour's phone-meter diff is already in `index.html`. Do not re-apply it. `phoneMic()` skips `openMeter()` on Android and iPhone, and `startRec()` calls `releaseMeter()` before `rec.start()`. The stale `onend` guard (`const mine = rec`) is also already there. Porcupine is still a pasted key and `BuiltInKeyword.Jarvis`. Not a free step. Do not sign up. Do not paste a key. Do not ship a synthetic Hey Bill model.

The wake open line is still two sentences: `` `${you()}. I'm here.` ``. The page-load bubble is already one sentence (`I'm here.`). Done wants the wake line to be one sentence too. A comma keeps the name and does not change listening, sleep, or the meter. Chrome's `SpeechRecognition.phrases` bias is experimental and not required for this. Do not add it. Do not add neon, khaki, or a new interruption.

`hearWake` already matches hey, ok, okay, or bare Bill. Sleep already sets `armed` and calls `startRec()`, so the mic returns to the wake word. Button, status, and hints still say Say Bill. That copy is not this hour's diff. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. iPhone Safari can keep `continuous` open and never return a final result, and an October 2026 WebKit bug says a later session in the same tab can hear silence. That is not a free one-line fix.

## READY FOR UPDATE

File: `index.html`

Smallest diff: make the wake open line one sentence. No new account. No model. No color change.

```diff
-  const open = `${you()}. I'm here.`;
+  const open = `${you()}, I'm here.`;
```

Still blocking a finished Bill: the button and hints still say Say Bill, so the page does not tell you to say Hey Bill even though the matcher already hears it. iPhone may still fail to return a final after the first session. Porcupine stays blocked on a key. Netlify team credits are exhausted, so a main commit does not by itself publish the live desk.
