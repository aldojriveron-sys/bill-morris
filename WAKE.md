# Phone open line is spoken off the tap.

Hour: 2026-10-06 21:15 EDT. Research only. Do not re-apply the 20:16 desktop hint. Commit `539ecfc` already says sleep drops back to the wake word on desktop. Do not re-apply the 19:15 edge-line skip. Main already says `Edge is ${channels.edge}.` in `replyTo()`. Do not re-apply the 16:13 timer gate. It is already in `wake()` and `speak()`. Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not call `SpeechRecognition.install()`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify. Do not add a silence watchdog. Do not pass a mic track into `rec.start()`. Do not change the wake regex. Do not change the phrase boost. Do not cancel the prime utterance in `arm()`.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. Phrase boost 8 stays inside 0.0–10.0. MDN still warns that 9 or 10 can make other phrases look like Hey Bill.

The page break this hour is the open line on a phone. `arm()` never calls `speechSynthesis`. `wake()` is what speaks, and a voice wake calls `wake()` from `onresult`, not from the tap. Chrome has required a user activation for `speechSynthesis.speak()` since M71. Mic permission is not that activation. Desktop Chrome keeps sticky activation from the Arm Bill click, so the open line can still be heard there. Chrome Android and iOS often drop `speak()` when the call is not in the tap, so the log shows the line and the phone stays quiet. That is the only code change this hour.

Prime it in `arm()`, before the Picovoice await, with a volume-0 space. Do not use `speak()`. That function sets `speaking` and aborts the recognizer. Do not cancel the prime in `arm()`. `wake()` already cancels before the open line. A volume-0 prime is not a second sentence and not an interruption. If a phone still ignores volume 0, this note does not claim the open line is fixed there.

The desktop hint now matches sleep. It still says the bars are mic gain. `startRec()` still calls `releaseMeter()` before `rec.start()`, on purpose, so Chrome does not open a second capture. The bars are flat once he is listening. Do not reopen the meter this hour. Do not edit that hint this hour.

Live https://bill-morris.netlify.app is still behind main. This run the served hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served page does not say Hey Bill. Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome still must not set `processLocally`. MDN says `start()` after `processLocally = true` throws `language-not-supported` if the en-US pack is not installed. Leaving the flag unset keeps the cloud path. Do not install a pack.

WebKit bug 326069 is still NEW. Latest comment is still the Radar importer at 2026-10-05 17:30 PDT, linking rdar://problem/189239983. No fix since. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

Main still has no `bill.jpg`, `bill-blink.jpg`, `bill-night.jpg`, or `icon.jpg`. The live deploy has the photo. A publish from a clean clone would drop it. Do not generate a stand-in. Do not upload.

## Still blocking

Netlify free credits still block publish. Live can stay behind main, including the old "say Bill" hint and the two-sentence open. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only.
Smallest diff, first lines of `arm()`, before the Picovoice await:

```
 async function arm() {
+  try {
+    const prime = new SpeechSynthesisUtterance(" ");
+    prime.volume = 0;
+    window.speechSynthesis.speak(prime);
+  } catch (e) {}
   if (picoKey.value.trim()) {
```

Do not edit the listener. Do not change the open line, the wake regex, the phrase boost, or the meter.
