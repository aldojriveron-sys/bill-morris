# A second failed start is still swallowed

Hour: 2026-10-05 18:13 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 17:15 start retry is already in `index.html` on main. `startRec` catches a thrown `start()`, waits 350ms, and calls `mine.start()` once. Do not re-apply it. The hidden-start guard, the visibility restart, the no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the inner catch. If that same-instance retry throws, the error is swallowed. The new recognizer never started, so it never fires `onend`. The aborted one's `onend` already returned because `rec !== mine`. Sleep, the end of a reply, and the no-speech restart all go through `startRec`. The desk can stay armed and deaf until the tab is hidden and shown again.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Main says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW). A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the inner catch of the 350ms retry. Do not touch the outer try, the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: if the same-instance retry throws, schedule one fresh `startRec`. Do not abort again inside the retry.

Replace:

```js
      try { mine.start(); if (awake) setStatus("live", "Listening"); } catch (err) {}
```

With:

```js
      try { mine.start(); if (awake) setStatus("live", "Listening"); }
      catch (err) {
        setTimeout(() => {
          if (rec !== mine || speaking || document.visibilityState === "hidden") return;
          if (!(awake || armed)) return;
          startRec();
        }, 700);
      }
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
