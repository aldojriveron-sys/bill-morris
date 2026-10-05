# Android Chrome rejects a hidden mic start

Hour: 2026-10-05 16:13 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 15:14 visibility restart is already in `index.html` on main. `document.visibilitychange` calls `startRec()` when the page is visible, armed or awake, and not speaking. Do not re-add it. The no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is a hidden start. Chromium issue 517429672 was marked fixed on 2026-06-16. On Android, the browser process now rejects a new Web Speech start if the page is hidden, and it ends an active session when the tab hides. `startRec` only bails for no recognizer, speaking, or not armed. Its `onend` path still calls `startRec` after 250ms. That timer can fire while the phone is locked or the app is switched away. `rec.start()` then fails, and the empty `catch` swallows it. The visibility listener already restarts on the way back. The missing line is to not start while hidden, so a rejected start does not sit in that catch.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Its open line is still two sentences. Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the guard at the top. One line. Do not touch the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: if the page is hidden, return before aborting or starting.

Replace:

```js
function startRec() {
  if (!SR || speaking) return;
  if (!awake && !armed) return;
  try { rec && rec.abort(); } catch (e) {}
```

With:

```js
function startRec() {
  if (!SR || speaking) return;
  if (!awake && !armed) return;
  if (document.visibilityState === "hidden") return;
  try { rec && rec.abort(); } catch (e) {}
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
