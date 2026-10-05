# Chrome start races the abort

Hour: 2026-10-05 17:15 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 16:13 hidden-start guard is already in `index.html` on main. `startRec` returns when `document.visibilityState === "hidden"`. Do not re-add it. The visibility restart, the no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the start that follows an abort. Chrome's Web Speech engine is one global singleton. Chromium commit 7910646, merged 2026-06-11 for M150, says a new `SpeechRecognition` that calls `start()` before the previous abort has finished raises `InvalidStateError`. `startRec` aborts the old recognizer, builds a new one, and calls `start()` in the same turn. The `catch` is empty. The old `onend` then sees `rec !== mine` and returns, so nothing schedules another start. Sleep, the end of a reply, and the no-speech restart all go through that path. The desk can stay armed and deaf until the tab is hidden and shown again.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Its open line is still two sentences, `` `${you()}. I'm here.` ``. It does not have the hidden guard. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW). A comment on 2026-10-05 says the same problem, and no workaround is documented. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the `rec.start()` catch at the bottom. Do not touch the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: if `start()` throws, retry once on the same instance after the singleton can finish the abort. Do not abort again inside the retry.

Replace:

```js
  if (phoneMic()) releaseMeter();
  try { rec.start(); if (awake) setStatus("live", "Listening"); } catch (e) {}
}
```

With:

```js
  if (phoneMic()) releaseMeter();
  try { rec.start(); if (awake) setStatus("live", "Listening"); }
  catch (e) {
    setTimeout(() => {
      if (rec !== mine || speaking || document.visibilityState === "hidden") return;
      if (!(awake || armed)) return;
      try { mine.start(); if (awake) setStatus("live", "Listening"); } catch (err) {}
    }, 350);
  }
}
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo. A second failed start is still swallowed.
