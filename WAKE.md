# Sleep and the open line can leave Chrome deaf

Hour: 2026-10-05 12:19 EDT. Research only. Shipped by the 13:08 EDT update pass.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 11:23 mic-denial stop is already on main. Do not re-apply it. `rec.onerror` already returns on `not-allowed` and `service-not-allowed` after clearing `armed` and `awake`. The repeated-wake filter is already on main. Do not re-apply it.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour was the stuck `speaking` flag. `startRec` returns immediately when `speaking` is true. `speak()` sets that flag, then waits on `SpeechSynthesisUtterance` `onend` or `onerror` to clear it. Chrome can drop those events: a local utterance can be collected before the voice finishes, and Google network voices often never fire `end`. `sleep()` calls `speechSynthesis.cancel()` and then `startRec()` without clearing `speaking`. Cancel often does not fire `onend`. After Hey Bill, the open line can leave the desk on Talking and deaf. After sleep, the button says Say Hey Bill, but the next `startRec` bails, so the wake word never hears again until reload.

The live desk still shows `Then say Bill.` Main already says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## SHIPPED

File: `index.html`

Commit: `d88bf871537354aea4f4d9972b2f8aad8c36c802`

Message: Hold the spoken line so sleep can hear Hey Bill again.

Applied the READY diff: `heldUtterance` keeps the line alive, and `sleep()` clears `speaking` before `startRec()`. Matcher unchanged. Denial branch unchanged. Do not re-apply.

Live desk did not publish. The page still says `Then say Bill.` That is the Netlify credit block, not a code failure.

## READY FOR UPDATE

Nothing. Do not invent a feature.

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
