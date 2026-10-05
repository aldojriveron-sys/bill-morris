# An aborted start can skip onend

Hour: 2026-10-05 19:14 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 18:13 same-instance retry is already applied. Commit `876089e` is on main. If the 350ms `mine.start()` throws, it schedules one fresh `startRec` after 700ms. Do not re-apply it. The hidden-start guard, the visibility restart, the no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the aborted path. `rec.onerror` returns immediately on `ev.error === "aborted"` and waits for `onend`. That is right when `speak()` or a new `startRec` aborted the session, because those paths already replace or restart it. It is wrong when Chrome aborts the current session and never fires `onend`. The new recognizer never started, so nothing else restarts it. Sleep, the end of a reply, and the wake listener all depend on that session. The desk can stay armed and deaf until the tab is hidden and shown again.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Main says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW, a second comment at 2026-10-05 07:25 PDT, no workaround). A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the `aborted` branch inside `rec.onerror`. Do not touch the outer start retry, the no-speech branch, the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: if this session is aborted and `onend` never marks it ended, schedule one fresh `startRec`. Do not restart an abort that `speak()` or a newer session already owns.

Replace:

```js
    if (ev.error === "aborted") return;
```

With:

```js
    if (ev.error === "aborted") {
      setTimeout(() => {
        if (ended || rec !== mine || speaking || document.visibilityState === "hidden") return;
        if (!(awake || armed)) return;
        startRec();
      }, 500);
      return;
    }
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
