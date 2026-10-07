# Desktop hint still says sleep toggles off.

Hour: 2026-10-06 20:16 EDT. Research only. Do not re-apply the 19:15 edge-line skip. Main already says `Edge is ${channels.edge}.` in `replyTo()`. Commit `a3515bbc` is already on main. Do not re-apply the 16:13 timer gate. It is already in `wake()` and `speak()`. Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not call `SpeechRecognition.install()`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify. Do not add a silence watchdog. Do not pass a mic track into `rec.start()`. Do not change the wake regex. Do not change the phrase boost.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. `handle` is already `async`. The script on main parses. Phrase boost 8 stays inside 0.0–10.0. MDN still warns that 9 or 10 can make other phrases look like Hey Bill.

The page break this hour is the desktop hint after wake. `wake()` sets the phone hint to "Say sleep to drop back to the wake word." The desktop branch still says "Say sleep to toggle off." Sleep does not disarm. The button click while awake also calls `sleep()`, and the button goes back to "Say Hey Bill". A desktop user is told the line turns off. It returns to the wake word. That is the only code change this hour.

Hiding the tab still aborts recognition and does not call `releaseMeter()`. The meter is desktop-only and already released at the start of `startRec()`. Do not add a second hide release this hour. Phone Chrome still ignores `continuous`; main already sets it false on Android only. Do not extend that flag to iPhone.

Live https://bill-morris.netlify.app is still behind main. This run the served hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served page does not say Hey Bill. Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome still must not set `processLocally`. MDN says `start()` after `processLocally = true` throws `language-not-supported` if the en-US pack is not installed. Chromium 542424205 is still an open P2: an on-device pack can abort recognition and then fail later sessions until the machine restarts. Leaving the flag unset keeps the cloud path. Do not install a pack.

WebKit bug 326069 is still NEW. Latest comment is still the Radar importer at 2026-10-05 17:30 PDT, linking rdar://problem/189239983. No fix since. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

Main still has no `bill.jpg`, `bill-blink.jpg`, `bill-night.jpg`, or `icon.jpg`. The live deploy has the photo. A publish from a clean clone would drop it. Do not generate a stand-in. Do not upload.

## Still blocking

Netlify free credits still block publish. Live can stay behind main, including the old "say Bill" hint and the two-sentence open. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only.
Smallest diff, inside `wake()`, the desktop hint:

```
- : "Hands-free. Bars are mic gain. The number is desk gain. Say sleep to toggle off.";
+ : "Hands-free. Bars are mic gain. The number is desk gain. Say sleep to drop back to the wake word.";
```

Do not edit the listener. Do not change the phone hint, the wake regex, the phrase boost, or the meter.
