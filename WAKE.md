# A late network error can abort the live session

Hour: 2026-10-05 21:15 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 20:16 result guard is already on main. `rec.onresult` starts with `if (rec !== mine || speaking) return;`. Do not re-apply it. The abort recovery, the same-instance retry, the hidden-start guard, the visibility restart, the no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the generic `onerror` fallthrough. `aborted` and `no-speech` already refuse to restart a replaced session. The last line does not. Chrome fires `network` when the cloud recognizer drops, which is the phone path, and it can also deliver that error after `startRec` has already aborted the session and assigned a new one. The timeout then calls `startRec()` with no `rec === mine` check and aborts the live listener. Sleep, the end of a reply, and the abort recovery all replace the session, so a late network error is on the normal path.

A late `not-allowed` could also disarm the desk. That is rarer than `network`. Not this hour.

The live desk is behind main. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW, no workaround). A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the last line of `rec.onerror`. Do not touch the abort branch, the no-speech branch, the not-allowed branch, the result guard, the start retry, the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: a network or audio-capture error may restart only its own session.

Replace:

```js
    if (awake || armed) setTimeout(startRec, 700);
```

With:

```js
    setTimeout(() => {
      if (ended || rec !== mine || speaking || document.visibilityState === "hidden") return;
      if (!(awake || armed)) return;
      startRec();
    }, 700);
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
