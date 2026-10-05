# Chrome restart race blocks Hey Bill

Hour: 2026-10-05 04:17 EDT. Research only. Not shipped by this pass.

## What I learned

The live desk at https://bill-morris.netlify.app is one page. One tap already calls `arm()`, which starts Chrome `webkitSpeechRecognition`. Sleep already calls `startRec()` again. `hearWake` already matches hey, ok, okay, or bare Bill:

```js
return /\b(hey |ok |okay )?bill\b/.test(transcript.toLowerCase());
```

The button and hint still say "Say Bill". The Porcupine branch loads `@picovoice/porcupine-web` and `BuiltInKeyword.Jarvis`, and it needs a pasted key. That is not a free next step. Do not sign up. Do not paste a key. Do not ship a synthetic Hey Bill model.

The free gap is the restart. `startRec` aborts the previous recognizer on every call. `abort()` fires `error` with `aborted`. The current `onerror` always schedules another `startRec` in 700ms, and `onend` schedules another in 250ms. Chrome then throws `InvalidStateError` on the second `start()`. The empty `catch` swallows it, so the mic looks armed and hears nothing. `interimResults` is false, so "Hey Bill" also waits for a final. Chrome often delays that final or splits the phrase.

Phone layout is not this hour's step. One `@media (max-width: 760px)` rule, safe-area padding, and the apple web-app metas are already on the page. Open line is still two sentences (`boss. I'm here.`). Leave that for a later pass.

## READY FOR UPDATE

File: `index.html`

Smallest diff: only `startRec`. Ignore `aborted`. Match the wake phrase on interim text. Keep commands on final text only. No new account. No model. No neon.

```diff
 function startRec() {
   if (!SR || speaking) return;
   if (!awake && !armed) return;
   try { rec && rec.abort(); } catch (e) {}
   rec = new SR();
   rec.lang = "en-US";
-  rec.interimResults = false;
+  rec.interimResults = true;
   rec.continuous = true;
   rec.onresult = (e) => {
-    const last = e.results[e.results.length - 1];
-    if (!last.isFinal) return;
-    const said = last[0].transcript;
-    if (!awake && hearWake(said)) { wake(); return; }
-    if (awake) handle(said);
+    let interim = "";
+    let finalSaid = "";
+    for (let i = e.resultIndex; i < e.results.length; i++) {
+      const chunk = e.results[i][0].transcript;
+      if (e.results[i].isFinal) finalSaid += chunk;
+      else interim += chunk;
+    }
+    if (!awake && hearWake(interim || finalSaid)) { wake(); return; }
+    if (awake && finalSaid) handle(finalSaid);
   };
-  rec.onerror = () => { if (awake || armed) setTimeout(startRec, 700); };
+  rec.onerror = (ev) => {
+    if (ev.error === "aborted" || ev.error === "no-speech") return;
+    if (awake || armed) setTimeout(startRec, 700);
+  };
   rec.onend = () => { if ((awake || armed) && !speaking) setTimeout(startRec, 250); };
   try { rec.start(); if (awake) setStatus("live", "Listening"); } catch (e) {}
 }
```

After that lands, the button can say Hey Bill. The matcher already hears it. Still blocking a finished Bill: this restart, the button copy, the two-sentence open line, and no proof yet that sleep returns to the wake word on a phone.
