# A replaced session can still wake him

Hour: 2026-10-05 20:16 EDT. Research only. Not applied.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 19:14 abort recovery is already applied. Commit `c09cc23` is on main. If Chrome aborts and `onend` never marks the session ended, it schedules one fresh `startRec` after 500ms, and skips an abort that `speak()` or a newer session already owns. Do not re-apply it. The same-instance retry, the hidden-start guard, the visibility restart, the no-speech restart, the speaking hold, and the repeated-wake filter are already on main. Do not re-apply them.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is a late result from a session `startRec` already replaced. `onresult` does not check `rec !== mine`. `startRec` aborts the current recognizer, then assigns a new one. Chrome can still deliver a final or interim on the aborted one. That callback can call `wake()` or `handle()` after the desk has moved on. Sleep, the end of a reply, and the new abort recovery all replace the session, so this race is now on the normal path. `handle()` already returns when `speaking` is true. It does not know the result came from a dead session.

Chrome contextual biasing (`SpeechRecognitionPhrase`, boost on "Hey Bill") is free and needs no key, but Chrome applies it on-device and can fire `phrases-not-supported` on the cloud recognizer this desk uses. Setting `processLocally` would change the phone path. Not this hour.

The live desk is behind main. Its hint is still `One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.` Main says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW, a second comment at 2026-10-05 07:25 PDT, no workaround). A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Function: `startRec`, the first lines of `rec.onresult`. Do not touch the abort branch, the no-speech branch, the start retry, the hidden guard, the visibility listener, the matcher, the open line, the phone rule, or `speak()`.

Smallest diff: ignore a result once this session is no longer current, or while Bill is talking.

Replace:

```js
  rec.onresult = (e) => {
    let interim = "";
    let finalSaid = "";
```

With:

```js
  rec.onresult = (e) => {
    if (rec !== mine || speaking) return;
    let interim = "";
    let finalSaid = "";
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
