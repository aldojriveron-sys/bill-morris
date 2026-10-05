# A dropped onend still leaves Chrome deaf after the open line

Hour: 2026-10-05 13:13 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 12:19 speaking-hold is already on main. Do not re-apply it. `heldUtterance` keeps the line alive, and `sleep()` already clears `speaking` before `startRec()`. The mic-denial stop and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the other half of the stuck `speaking` flag. `startRec` still returns immediately when `speaking` is true. `speak()` sets that flag and clears it only in `SpeechSynthesisUtterance` `onend` or `onerror`. Holding the utterance stops Chrome from collecting it. It does not make a Google network voice fire `end`. Those voices often never fire `onend` or `onerror` (Chromium 409717085, still the same class of failure in 2025). `speak()` also calls `speechSynthesis.cancel()` and then `speak()` on the same turn. Chrome can swallow that utterance and never fire `end` either. After Hey Bill, the open line can leave the pill on Talking and the next `startRec` bailing. Sleep can recover only if the user taps the button, because `sleep()` clears the flag itself. Waiting for the line to finish does not.

The recognizer is also left running while Bill talks. `handle()` returns early when `speaking` is true, so the echo is ignored, but Chrome can abort that session and leave it dead. Aborting it at the start of `speak()` is the same pattern `startRec` already uses.

The live desk still shows `Then say Bill.` Main already says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `speak`

Smallest diff: abort the live recognizer, speak on the next tick after `cancel()`, and clear `speaking` on a timer if `onend` never comes. Do not change the matcher, the open line, the phone rule, or the denial branch.

Replace:

```js
function speak(text) {
  return new Promise(resolve => {
    speaking = true;
    setStatus("talk", "Talking");
    window.speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    heldUtterance = u;
    const v = voices.find(x => x.name === voiceSel.value);
    if (v) u.voice = v;
    u.rate = 1.01; u.pitch = 0.9;
    const done = () => {
      speaking = false;
      if (heldUtterance === u) heldUtterance = null;
      resolve();
    };
    u.onend = done;
    u.onerror = done;
    window.speechSynthesis.speak(u);
  });
}
```

With:

```js
function speak(text) {
  return new Promise(resolve => {
    speaking = true;
    setStatus("talk", "Talking");
    try { rec && rec.abort(); } catch (e) {}
    window.speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    heldUtterance = u;
    const v = voices.find(x => x.name === voiceSel.value);
    if (v) u.voice = v;
    u.rate = 1.01; u.pitch = 0.9;
    let settled = false;
    const done = () => {
      if (settled) return;
      settled = true;
      clearTimeout(watch);
      speaking = false;
      if (heldUtterance === u) heldUtterance = null;
      resolve();
    };
    const watch = setTimeout(done, Math.min(12000, 1600 + text.length * 90));
    u.onend = done;
    u.onerror = done;
    setTimeout(() => window.speechSynthesis.speak(u), 40);
  });
}
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
