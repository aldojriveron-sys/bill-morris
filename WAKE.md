# Stale onend still cuts Hey Bill

Hour: 2026-10-05 05:13 EDT. Research only. Not shipped by this pass.

## What I learned

The brief repo name `aldojjriveron02/bill-morris` returns 404. The desk that can take a free push is `aldojriveron-sys/bill-morris` on main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify.

`index.html` on main already has the last note's `interimResults = true` and the `aborted` / `no-speech` ignore. I am not calling that a ship from the update machine. The restart is still incomplete.

`startRec` still aborts the previous recognizer, then builds a new one. Chrome fires `end` after `abort`. The aborted object's `onend` does not check that it is still `rec`, so 250ms later it calls `startRec` again. That second call aborts the live recognizer. A Hey Bill that landed in the gap is dropped, and the empty `catch` on `start()` still hides `InvalidStateError`. Continuous Chrome recognition also ends on its own after a few seconds of silence, so a real `onend` must still restart. Only the stale one should be ignored.

Porcupine is still a pasted key and `BuiltInKeyword.Jarvis`. Not a free step. Do not sign up. Do not paste a key. Do not ship a synthetic Hey Bill model.

Phone layout is already one `@media (max-width: 760px)` rule, safe-area padding, and the apple web-app metas. Leave it. Open line is still two sentences: `` `${you()}. I'm here.` ``. Button and hints still say Say Bill. The matcher already hears hey, ok, okay, or bare Bill.

After this lands, the phone mic is the next block. `wake()` calls `openMeter()`, which holds `getUserMedia` for the bars, then `startRec()`. On Android Chrome the mic is one owner at a time, so SpeechRecognition can go silent once the meter stream is open. That is not this hour's diff.

## READY FOR UPDATE

File: `index.html`

Smallest diff: only `startRec`. Tag the recognizer this call created. Ignore `onend` if it is no longer `rec`. No new account. No model. No neon.

```diff
 function startRec() {
   if (!SR || speaking) return;
   if (!awake && !armed) return;
   try { rec && rec.abort(); } catch (e) {}
   rec = new SR();
+  const mine = rec;
   rec.lang = "en-US";
   rec.interimResults = true;
   rec.continuous = true;
   rec.onresult = (e) => {
     let interim = "";
     let finalSaid = "";
     for (let i = e.resultIndex; i < e.results.length; i++) {
       const chunk = e.results[i][0].transcript;
       if (e.results[i].isFinal) finalSaid += chunk;
       else interim += chunk;
     }
     if (!awake && hearWake(interim || finalSaid)) { wake(); return; }
     if (awake && finalSaid) handle(finalSaid);
   };
   rec.onerror = (ev) => {
     if (ev.error === "aborted" || ev.error === "no-speech") return;
     if (awake || armed) setTimeout(startRec, 700);
   };
-  rec.onend = () => { if ((awake || armed) && !speaking) setTimeout(startRec, 250); };
+  rec.onend = () => {
+    if (rec !== mine) return;
+    if ((awake || armed) && !speaking) setTimeout(startRec, 250);
+  };
   try { rec.start(); if (awake) setStatus("live", "Listening"); } catch (e) {}
 }
```

Still blocking a finished Bill: this stale restart, the button still saying Say Bill, the two-sentence open line, and the meter stream that can take the phone mic after wake so sleep never hears the wake word again.
