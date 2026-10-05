# Mic denial still restarts Chrome listening

Hour: 2026-10-05 11:23 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 10:23 repeated-wake filter is already on main. Do not re-apply it. `index.html` `onresult` already skips one or more copies of the wake phrase:

```js
if (awake && finalSaid && !/^\s*((hey|ok|okay)?[, ]*bill[.!,?\s]*)+$/i.test(finalSaid)) handle(finalSaid);
```

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the denial restart. `onerror` returns only on `aborted` and `no-speech`, then schedules `startRec` in 700ms for every other error, including `not-allowed`. `onend` also schedules `startRec` in 250ms whenever `armed` or `awake` is set. A denied mic therefore queues two restarts. MDN defines `not-allowed` as the user agent blocking speech input for security, privacy, or user preference, and `service-not-allowed` as the same class of block on the recognition service. Restarting does not grant the mic. It can re-prompt or spin. `network` should still restart. Do not touch the wake matcher.

The live desk still shows `Then say Bill.` Main already says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW, no comments): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. All iPhone browsers use WebKit. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: stop the recognition loop on a mic denial, and leave the button on Arm Bill so one tap can try again after the user allows the mic. No new account. No model. No color change. Matcher unchanged.

```diff
   rec.onerror = (ev) => {
+    if (ev.error === "not-allowed" || ev.error === "service-not-allowed") {
+      armed = false;
+      awake = false;
+      wakeBtn.textContent = "Arm Bill";
+      wakeBtn.classList.remove("off");
+      setStatus("", "Asleep");
+      hint.textContent = "Mic blocked. Allow it, then tap Arm Bill.";
+      return;
+    }
     if (ev.error === "aborted" || ev.error === "no-speech") return;
     if (awake || armed) setTimeout(startRec, 700);
   };
   rec.onend = () => {
     if (rec !== mine) return;
-    if ((awake || armed) && !speaking) setTimeout(startRec, 250);
+    if ((awake || armed) && !speaking) setTimeout(startRec, 250);
   };
```

The `onend` line does not change. Clearing `armed` and `awake` in the denial branch is what stops the 250ms restart. `network` still restarts.

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
