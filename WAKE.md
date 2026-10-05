# Hey Bill still falls through as a command

Hour: 2026-10-05 09:31 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 08:30 copy diff is already on main. `index.html` has zero `Say Bill` strings. The idle hint, the armed button, the status pill, and the sleep hint already say Hey Bill. Do not re-apply that diff.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`, so Hey Bill, Ok Bill, Okay Bill, and bare Bill wake him. `sleep()` sets `armed` and calls `startRec()`, so sleep returns to the wake word. The open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage: Chrome often finalizes the same utterance that woke him. `onresult` calls `wake()` on the interim, and `wake()` sets `awake` immediately. The later final of that same breath then takes the awake branch and `handle()` treats "Hey Bill" as a command. Chrome also punctuates it as "Hey, Bill". The current wake-only check does not exist, so the open line and a reply can both talk. A command with extra words should still be handled.

The live desk still shows "Then say Bill." Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: ignore a final that is only the wake phrase, including Chrome's comma. No new account. No model. No color change. Matcher unchanged.

```diff
-    if (awake && finalSaid) handle(finalSaid);
+    if (awake && finalSaid && !/^\s*(hey|ok|okay)?[, ]*bill[.!,?\s]*$/i.test(finalSaid)) handle(finalSaid);
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
