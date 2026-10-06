# A dead mic can keep restarting with no hint

Hour: 2026-10-05 23:13 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 22:13 permission guard is already on main, commit `7857846`. The `not-allowed` branch starts with `if (rec !== mine) return`. Do not re-apply it. The abort recovery, the same-instance retry, the hidden-start guard, the visibility restart, the no-speech restart, the speaking hold, the repeated-wake filter, and the network fallthrough are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is `audio-capture`. `aborted`, `no-speech`, `not-allowed`, and the network fallthrough already have their own branches. Capture failure does not. It falls through to the 700ms restart. The desk stays on Say Hey Bill or Listening, and the hint never changes. A pulled mic, a taken device, or a dead audio service then looks armed.

Chromium 569507846 is still open. Chrome 154.0.8037.97 and 154.0.8037.98 on Windows can crash the sandboxed AudioService at startup. No fix landed. The command-line workaround is not a page diff. A restart loop cannot repair that crash. A hint can at least say the mic dropped.

WebKit bug 326069 is still NEW. Radar `rdar://problem/189239983` was filed 2026-10-05 17:30 PDT. No workaround accepted. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

The live desk is behind main. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by that WebKit bug. Porcupine stays blocked on a key.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, `rec.onerror`, before the network fallthrough. Do not touch the abort branch, the no-speech branch, the not-allowed branch, the network fallthrough, the result guard, the start retry, the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: a capture error may restart only its own session, and the hint says the mic dropped.

Replace:

```js
    if (ev.error === "no-speech") {
      setTimeout(() => {
        if (!ended && rec === mine && (awake || armed) && !speaking) startRec();
      }, 400);
      return;
    }
    setTimeout(() => {
```

With:

```js
    if (ev.error === "no-speech") {
      setTimeout(() => {
        if (!ended && rec === mine && (awake || armed) && !speaking) startRec();
      }, 400);
      return;
    }
    if (ev.error === "audio-capture") {
      if (rec !== mine) return;
      hint.textContent = "Mic dropped. Tap Arm Bill if it stays quiet.";
      setTimeout(() => {
        if (ended || rec !== mine || speaking || document.visibilityState === "hidden") return;
        if (!(awake || armed)) return;
        startRec();
      }, 700);
      return;
    }
    setTimeout(() => {
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
