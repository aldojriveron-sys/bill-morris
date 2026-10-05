# Android still turns a repeated Hey Bill into a command

Hour: 2026-10-05 10:23 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 09:31 wake-only filter is already on main. Do not re-apply it. `index.html` `onresult` already skips a final that is only one wake phrase, including Chrome's comma:

```js
if (awake && finalSaid && !/^\s*(hey|ok|okay)?[, ]*bill[.!,?\s]*$/i.test(finalSaid)) handle(finalSaid);
```

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the phone mic path. `startRec` sets `interimResults = true` and `continuous = true`. Chromium 40272768 (Android Chrome, still open) marks interim copies final, and a later event can deliver the same phrase twice. The current regex allows one wake phrase only, so a final of `Hey Bill Hey Bill` fails the test and `handle()` treats the wake word as a command. A command with extra words, such as `Hey Bill what's the gain`, must still be handled. Desktop `Hey, Bill` is already covered. Do not drop `confidence === 0` finals: desktop Chrome sometimes reports 0 on a real final.

The live desk still shows `Then say Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. All iPhone browsers use WebKit. That is not a free one-line diff.

`onerror` still restarts on `not-allowed`. That is a later pass. Do not stack it on this diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: treat one or more copies of the wake phrase as wake-only, so Android's repeated final does not become a command. No new account. No model. No color change. Matcher unchanged.

```diff
-    if (awake && finalSaid && !/^\s*(hey|ok|okay)?[, ]*bill[.!,?\s]*$/i.test(finalSaid)) handle(finalSaid);
+    if (awake && finalSaid && !/^\s*((hey|ok|okay)?[, ]*bill[.!,?\s]*)+$/i.test(finalSaid)) handle(finalSaid);
```

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
