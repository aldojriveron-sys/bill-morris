# Sleep and the open line can leave Chrome deaf

Hour: 2026-10-05 12:19 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The 11:23 mic-denial stop is already on main. Do not re-apply it. `rec.onerror` already returns on `not-allowed` and `service-not-allowed` after clearing `armed` and `awake`. The repeated-wake filter is already on main. Do not re-apply it.

Done checks on main, not on the live desk: one tap calls `arm()` and `startRec()` on Chrome `SpeechRecognition`. `hearWake` is still `/\b(hey |ok |okay )?bill\b/`. `sleep()` sets `armed` and calls `startRec()`. The spoken open line is already one sentence, `` `${you()}, I'm here.` ``. Phone layout is still the one `@media (max-width: 760px)` rule. Leave it. Do not ship a synthetic Hey Bill model. Do not paste a Porcupine key. Do not add neon, khaki, or a new interruption.

Breakage this hour is the stuck `speaking` flag. `startRec` returns immediately when `speaking` is true. `speak()` sets that flag, then waits on `SpeechSynthesisUtterance` `onend` or `onerror` to clear it. Chrome can drop those events: a local utterance can be collected before the voice finishes, and Google network voices often never fire `end`. `sleep()` calls `speechSynthesis.cancel()` and then `startRec()` without clearing `speaking`. Cancel often does not fire `onend`. After Hey Bill, the open line can leave the desk on Talking and deaf. After sleep, the button says Say Hey Bill, but the next `startRec` bails, so the wake word never hears again until reload.

The live desk still shows `Then say Bill.` Main already says `Then say Hey Bill.` Netlify credits are exhausted, so main does not publish. iPhone is still blocked by WebKit bug 326069 (reported 2026-10-02, still NEW): on iOS 27, SpeechRecognition hears only the first session in a tab. A new instance does not fix it. Reloading the tab does not fix it. Closing the tab does. That is not a free one-line diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: keep the utterance alive, and clear `speaking` before sleep restarts Chrome. No new account. No model. No color change. Matcher unchanged. Denial branch unchanged.

```diff
 let audioCtx, analyser, micStream, meterTimer;
+let heldUtterance = null;

 function speak(text) {
   return new Promise(resolve => {
     speaking = true;
     setStatus("talk", "Talking");
     window.speechSynthesis.cancel();
     const u = new SpeechSynthesisUtterance(text);
+    heldUtterance = u;
     const v = voices.find(x => x.name === voiceSel.value);
     if (v) u.voice = v;
     u.rate = 1.01; u.pitch = 0.9;
-    u.onend = () => { speaking = false; resolve(); };
-    u.onerror = () => { speaking = false; resolve(); };
+    const done = () => {
+      speaking = false;
+      if (heldUtterance === u) heldUtterance = null;
+      resolve();
+    };
+    u.onend = done;
+    u.onerror = done;
     window.speechSynthesis.speak(u);
   });
 }
 function sleep() {
   awake = false;
   armed = true;
+  speaking = false;
   window.speechSynthesis.cancel();
   wakeBtn.textContent = "Say Hey Bill";
   wakeBtn.classList.remove("off");
   setStatus("live", "Say Hey Bill");
   hint.textContent = "Asleep. Say Hey Bill when you want him.";
   startRec();
 }
```

`heldUtterance` is what stops Chrome from collecting the line before `onend`. Clearing `speaking` in `sleep()` is what lets the wake word start again after cancel. Do not touch `hearWake`.

Still blocking a finished Bill: the live desk will not show main until Netlify credits return, because a main commit does not publish. iPhone may still hear only the first session in a tab. Porcupine stays blocked on a key. The brief owner `aldojjriveron02` still has no repo.
