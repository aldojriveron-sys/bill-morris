# Resume speech on the arm tap so the phone prime is not stuck.

Hour: 2026-10-06 22:15 EDT. Research only. Do not re-apply the 21:15 silent prime. Commit `59d7073` already speaks a volume-0 space at the top of `arm()`. Do not re-apply the 20:16 desktop hint. Commit `539ecfc` already says sleep drops back to the wake word on desktop. Do not re-apply the 19:15 edge-line skip. Main already says `Edge is ${channels.edge}.` in `replyTo()`. Do not re-apply the 16:13 timer gate. It is already in `wake()` and `speak()`. Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not call `SpeechRecognition.install()`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify. Do not add a silence watchdog. Do not call `pause()` on the phone. Do not pass a mic track into `rec.start()`. Do not change the wake regex. Do not change the phrase boost. Do not cancel the prime utterance in `arm()`.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. Phrase boost 8 stays inside 0.0–10.0. MDN still warns that 9 or 10 can make other phrases look like Hey Bill.

The page break this hour is the prime that just landed. `arm()` queues a volume-0 space and never calls `speechSynthesis.resume()`. Chrome Android can report `paused` false while the queue is stuck, and later `speak()` calls only go pending. The usual free unblock is `resume()` in the same tap, before `speak()`. A 14-second `pause()` then `resume()` watchdog is a different bug, the long-Google-voice halt, and on Android `pause()` ends the current utterance. Do not add that watchdog. Do not call `pause()` here.

Hold the prime on `heldUtterance` so the arm function returning does not drop the only reference before the engine starts. `wake()` already sets `heldUtterance` null before the open line, and `speak()` already cancels before it talks. Do not cancel the prime in `arm()`. This note does not claim the open line is fixed on a phone if volume 0 is ignored.

The desktop hint now matches sleep. It still says the bars are mic gain. `startRec()` still calls `releaseMeter()` before `rec.start()`, on purpose, so Chrome does not open a second capture. The bars are flat once he is listening. Do not reopen the meter this hour. Do not edit that hint this hour.

Live https://bill-morris.netlify.app is still behind main. This run the served hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served page has no prime. Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome still must not set `processLocally`. MDN says `start()` after `processLocally = true` throws `language-not-supported` if the en-US pack is not installed. Leaving the flag unset keeps the cloud path. Do not install a pack.

WebKit bug 326069 is still NEW. Latest comment is still the Radar importer at 2026-10-05 17:30 PDT, linking rdar://problem/189239983. No fix since. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

Main still has no `bill.jpg`, `bill-blink.jpg`, `bill-night.jpg`, or `icon.jpg`. The live deploy has the photo. A publish from a clean clone would drop it. Do not generate a stand-in. Do not upload.

## Still blocking

Netlify free credits still block publish. Live can stay behind main, including the old "say Bill" hint and the two-sentence open. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only.
Smallest diff, inside the existing try at the top of `arm()`, before the volume-0 speak:

```
   try {
+    window.speechSynthesis.resume();
     const prime = new SpeechSynthesisUtterance(" ");
     prime.volume = 0;
+    heldUtterance = prime;
     window.speechSynthesis.speak(prime);
   } catch (e) {}
```

Do not edit the listener. Do not change the open line, the wake regex, the phrase boost, or the meter. Do not cancel the prime.
