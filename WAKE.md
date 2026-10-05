# A hidden tab can leave Chrome armed and deaf

Hour: 2026-10-05 15:14 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 14:14 no-speech restart is already in `index.html` on main. `startRec` ignores `aborted`, stops on mic denial, and if `no-speech` arrives with no `onend`, it calls `startRec` again after 400ms. Do not re-apply it. The speaking hold and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

A JSGF grammar will not make Hey Bill more reliable. MDN marks `SpeechGrammarList` deprecated, and the grammar concept no longer affects the recognition service. Chrome's cloud recognizer ignores it. Do not add one.

Breakage this hour is the mic after the page is hidden. Chrome suspends work when the tab is in the background, and a phone locks or switches apps the same way. `startRec` only restarts from `onend`, `no-speech`, or another error. If that event was throttled or dropped while hidden, the pill can still say Say Hey Bill and the next Hey Bill never arrives. Coming back to a visible page is a free, local signal. `startRec` already aborts the old instance, skips while `speaking`, and no-ops unless armed or awake.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Its open line is still two sentences, `` `${you()}. I'm here.` ``. Its `onerror` restarts on every error, including mic denial. Its `speak()` has no hold and no timer. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: after `startRec`, once. Not inside it.

Smallest diff: when the page becomes visible again, restart only if Bill is armed or awake and not speaking. Do not change the matcher, the open line, the phone rule, `speak()`, or the no-speech timer.

Replace:

```js
  try { rec.start(); if (awake) setStatus("live", "Listening"); } catch (e) {}
}
function sleep() {
```

With:

```js
  try { rec.start(); if (awake) setStatus("live", "Listening"); } catch (e) {}
}
document.addEventListener("visibilitychange", () => {
  if (document.visibilityState !== "visible") return;
  if ((awake || armed) && !speaking) startRec();
});
function sleep() {
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
