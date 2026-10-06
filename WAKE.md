# A late permission error can disarm the live session

Hour: 2026-10-05 22:13 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 21:15 network guard is already on main. The last line of `rec.onerror` is the guarded timeout, not `if (awake || armed) setTimeout(startRec, 700)`. Do not re-apply it. The abort recovery, the same-instance retry, the hidden-start guard, the visibility restart, the no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the `not-allowed` branch. `aborted`, `no-speech`, and the network fallthrough already refuse to restart a replaced session. Permission does not. `startRec` aborts the old recognizer before it assigns the new one. Sleep, the end of a reply, and the abort recovery all do that. Chrome can still deliver `not-allowed` or `service-not-allowed` on the aborted instance. That branch sets `armed` and `awake` to false with no `rec === mine` check, so a late error puts the live desk back on Arm Bill.

WebKit bug 326069 is still NEW. Last comment 2026-10-05 07:25 PDT was only "same problem here." No workaround accepted. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

The live desk is behind main. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by that WebKit bug. Porcupine stays blocked on a key.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the `not-allowed` branch of `rec.onerror`. Do not touch the abort branch, the no-speech branch, the network fallthrough, the result guard, the start retry, the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: a permission error may disarm only its own session.

Replace:

```js
    if (ev.error === "not-allowed" || ev.error === "service-not-allowed") {
      armed = false;
      awake = false;
      wakeBtn.textContent = "Arm Bill";
      wakeBtn.classList.remove("off");
      setStatus("", "Asleep");
      hint.textContent = "Mic blocked. Allow it, then tap Arm Bill.";
      return;
    }
```

With:

```js
    if (ev.error === "not-allowed" || ev.error === "service-not-allowed") {
      if (rec !== mine) return;
      armed = false;
      awake = false;
      wakeBtn.textContent = "Arm Bill";
      wakeBtn.classList.remove("off");
      setStatus("", "Asleep");
      hint.textContent = "Mic blocked. Allow it, then tap Arm Bill.";
      return;
    }
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
