# Who-are-you line says Edge is undefined. No listening diff.

Hour: 2026-10-06 18:17 EDT. Research only. Do not re-apply the 16:13 timer gate. It is already in `wake()` and `speak()` on main (`heldUtterance = null` after `speaking = true`, and the 320ms timer clears `speaking` only if `heldUtterance === u`). Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not call `SpeechRecognition.install()`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify. Do not add a silence watchdog. Do not pass a mic track into `rec.start()`.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. No layout diff. No phrase diff. Phrase boost 8 stays inside 0.0–10.0.

The page line is broken. `replyTo()` on "who are you" says `Edge is ${lv.edge}`. `level()` returns a rank from `LEVELS`, and those objects only have `at` and `name`. `lv.edge` is always undefined, so Bill says "Edge is undefined." The edge number lives on `channels.edge`. That is the only code change this hour.

Live https://bill-morris.netlify.app is still behind main. At 18:17 the hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served script has neither `heldUtterance` nor `releaseMeter`. It does still contain `lv.edge`. Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome still must not set `processLocally`. MDN says `start()` after `processLocally = true` throws `language-not-supported` if the en-US pack is not installed. Chromium 542424205 (opened 2026-08-04, still no fix) says an on-device pack can abort recognition and then fail later sessions until the machine restarts. Leaving the flag unset keeps the cloud path. Do not install a pack.

WebKit bug 326069 is still NEW. Latest comment is still the Radar importer at 2026-10-05 17:30 PDT. No fix since. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

Main still has no `bill.jpg`, `bill-blink.jpg`, `bill-night.jpg`, or `icon.jpg`. The live deploy has the photo. A publish from a clean clone would drop it. Do not generate a stand-in. Do not upload.

## Still blocking

Netlify free credits still block publish. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `replyTo()`, the who-are-you return:

```
- Right now I'm ${lv.name}. Edge is ${lv.edge}.`;
+ Right now I'm ${lv.name}. Edge is ${channels.edge}.`;
```

Do not edit the listener. Do not change the hint, the wake regex, the phrase boost, or the meter. The update pass should apply that one token and leave the rest of main alone.
