# A no-speech error can leave Chrome armed and deaf

Hour: 2026-10-05 14:14 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 13:13 speaking-hold is already in `index.html` on main. `speak()` aborts the recognizer, holds the utterance, waits 40ms after `cancel()`, and clears `speaking` on a timer if `onend` never comes. Do not re-apply it. The mic-denial stop and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the wake listener, not the open line. `startRec` still returns early on `no-speech` and relies on `onend` to restart:

```js
if (ev.error === "aborted" || ev.error === "no-speech") return;
```

Chrome fires `no-speech` when the armed session hears nothing, then is supposed to fire `end`. The same class of dropped `end` already needed a timer on the speech-synthesis side (Chromium 409717085, Google voices). If recognition drops `end` after `no-speech`, the pill stays on Say Hey Bill and the next `startRec` never runs. Our own `abort()` during `speak()` must still be ignored, or the timer and the abort will fight.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Its `speak()` has no hold and no timer. Its `onerror` restarts on every error, including mic denial. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`

Smallest diff: restart after `no-speech` only if `onend` never came. Keep the denial stop. Keep ignoring `aborted`. Do not change the matcher, the open line, the phone rule, or `speak()`.

Replace:

```js
  rec.onerror = (ev) => {
    if (ev.error === "not-allowed" || ev.error === "service-not-allowed") {
      armed = false;
      awake = false;
      wakeBtn.textContent = "Arm Bill";
      wakeBtn.classList.remove("off");
      setStatus("", "Asleep");
      hint.textContent = "Mic blocked. Allow it, then tap Arm Bill.";
      return;
    }
    if (ev.error === "aborted" || ev.error === "no-speech") return;
    if (awake || armed) setTimeout(startRec, 700);
  };
  rec.onend = () => {
    if (rec !== mine) return;
    if ((awake || armed) && !speaking) setTimeout(startRec, 250);
  };
```

With:

```js
  let ended = false;
  rec.onerror = (ev) => {
    if (ev.error === "not-allowed" || ev.error === "service-not-allowed") {
      armed = false;
      awake = false;
      wakeBtn.textContent = "Arm Bill";
      wakeBtn.classList.remove("off");
      setStatus("", "Asleep");
      hint.textContent = "Mic blocked. Allow it, then tap Arm Bill.";
      return;
    }
    if (ev.error === "aborted") return;
    if (ev.error === "no-speech") {
      setTimeout(() => {
        if (!ended && rec === mine && (awake || armed) && !speaking) startRec();
      }, 400);
      return;
    }
    if (awake || armed) setTimeout(startRec, 700);
  };
  rec.onend = () => {
    ended = true;
    if (rec !== mine) return;
    if ((awake || armed) && !speaking) setTimeout(startRec, 250);
  };
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
