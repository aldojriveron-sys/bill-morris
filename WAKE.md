# Phone meter still takes the mic

Hour: 2026-10-05 06:17 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo name `aldojjriveron02/bill-morris` still 404s. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

The last note's stale `onend` guard is already in `index.html` on main: `const mine = rec` and `if (rec !== mine) return`. Do not re-apply that diff. Porcupine is still a pasted key and `BuiltInKeyword.Jarvis`. Not a free step. Do not sign up. Do not paste a key. Do not ship a synthetic Hey Bill model.

The remaining Chrome listening break is the meter. `arm()` only calls `startRec()`. `wake()` then `await openMeter()`, which keeps a `getUserMedia` stream for the bars, then `startRec()`. `openMeter` returns early if `micStream` is set, and `sleep()` never stops those tracks. Android Chrome will not run `getUserMedia` and `SpeechRecognition` on the mic at the same time. Whichever capture started first wins; the other gets silence or `audio-capture` / `not-allowed`. A 2025 Chrome community answer says the same: MediaRecorder and SpeechRecognition cannot run together on Android browsers. Desktop Chrome can keep both. So after the first wake, the phone mic stays with the bars, sleep returns to the wake-word label, and Hey Bill has nothing to hear.

Phone layout is already the one `@media (max-width: 760px)` rule. Leave it. Open line is still two sentences: `` `${you()}. I'm here.` ``. Button and hints still say Say Bill. The matcher already hears hey, ok, okay, or bare Bill. Not this hour's diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: on a phone, do not open the meter, and drop any held tracks before `rec.start()`. Desktop bars stay. No new account. No model. No neon.

```diff
 let audioCtx, analyser, micStream, meterTimer;
+
+function phoneMic() {
+  return /Android|iPhone|iPad|iPod/i.test(navigator.userAgent);
+}
+function releaseMeter() {
+  if (meterTimer) { clearInterval(meterTimer); meterTimer = null; }
+  if (micStream) {
+    micStream.getTracks().forEach(t => t.stop());
+    micStream = null;
+  }
+  if (audioCtx) {
+    try { audioCtx.close(); } catch (e) {}
+    audioCtx = null;
+    analyser = null;
+  }
+}
 
 async function openMeter() {
+  if (phoneMic()) return;
   if (micStream) return;
   try {
     micStream = await navigator.mediaDevices.getUserMedia({ audio: true });
@@
   rec.onend = () => {
     if (rec !== mine) return;
     if ((awake || armed) && !speaking) setTimeout(startRec, 250);
   };
+  if (phoneMic()) releaseMeter();
   try { rec.start(); if (awake) setStatus("live", "Listening"); } catch (e) {}
 }
@@
   await speak(open);
-  hint.textContent = "Hands-free. Bars are mic gain. The number is desk gain. Say sleep to toggle off.";
+  hint.textContent = phoneMic()
+    ? "Hands-free. Say sleep to drop back to the wake word."
+    : "Hands-free. Bars are mic gain. The number is desk gain. Say sleep to toggle off.";
   startRec();
 }
```

If a phone already holds the stream, `track.stop()` then `rec.start()` can still lose the first grab. The existing `onerror` retry at 700ms covers `audio-capture`. Do not add a second timer.

Still blocking a finished Bill: this phone meter hold, the button still saying Say Bill, and the two-sentence open line. iPhone may still lack a continuous Chrome-style recognizer after the release. Porcupine stays blocked on a key.
