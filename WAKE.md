# Page still says Say Bill

Hour: 2026-10-05 08:30 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

Last hour's open-line diff is already on main. Commit `0b1c6def` ("Make the wake open line one sentence.") changed `index.html` to `` `${you()}, I'm here.` ``. Do not re-apply it. The page-load bubble is already `I'm here.` Phone meter skip and the stale `onend` guard are already in. Leave them.

`hearWake` already matches hey, ok, okay, or bare Bill: `/\b(hey |ok |okay )?bill\b/`. One tap still arms Chrome SpeechRecognition. Sleep still sets `armed` and calls `startRec()`, so the mic returns to the wake word. The page does not say that. The idle hint, the armed button, the status pill, and the sleep hint all say Say Bill. That is the gap between a working matcher and a finished Hey Bill.

Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add `SpeechRecognition.phrases`. Do not add neon, khaki, or a new interruption. Bare Bill can stay a match; the copy should tell the truth about Hey Bill.

Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02): on iOS 27, SpeechRecognition hears only the first session in a tab, and a later session gets silence. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: say Hey Bill in the wake copy. No new account. No model. No color change. Matcher unchanged.

```diff
-    <div class="hint" id="hint">One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word.</div>
+    <div class="hint" id="hint">One tap arms the mic. Then say Hey Bill. Say sleep to drop back to the wake word.</div>
```

```diff
-  wakeBtn.textContent = "Say Bill";
-  setStatus("live", "Say Bill");
-  hint.textContent = "Mic is armed. Say Bill to wake him. Say sleep to drop back to the wake word.";
+  wakeBtn.textContent = "Say Hey Bill";
+  setStatus("live", "Say Hey Bill");
+  hint.textContent = "Mic is armed. Say Hey Bill to wake him. Say sleep to drop back to the wake word.";
```

```diff
-    hint.textContent = "Porcupine didn't start. Staying on Say Bill. " + (e.message || "");
+    hint.textContent = "Porcupine didn't start. Staying on Say Hey Bill. " + (e.message || "");
```

```diff
-  wakeBtn.textContent = "Say Bill";
-  wakeBtn.classList.remove("off");
-  setStatus("live", "Say Bill");
-  hint.textContent = "Asleep. Say Bill when you want him.";
+  wakeBtn.textContent = "Say Hey Bill";
+  wakeBtn.classList.remove("off");
+  setStatus("live", "Say Hey Bill");
+  hint.textContent = "Asleep. Say Hey Bill when you want him.";
```

Still blocking a finished Bill: the live desk will not show this until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
