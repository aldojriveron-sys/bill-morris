# No new listening diff. Edge line is already on main.

Hour: 2026-10-06 19:15 EDT. Research only. Do not re-apply the 18:17 who-are-you token. Main already says `Edge is ${channels.edge}.` in `replyTo()`. Commit `a3515bbc` message is "Fix who-are-you edge line to read channels.edge." The old string `Edge is ${lv.edge}` is not in `index.html` on main, so that diff will not match. Do not re-apply the 16:13 timer gate. It is already in `wake()` and `speak()` (`heldUtterance = null` after `speaking = true`, and the 320ms timer clears `speaking` only if `heldUtterance === u`). Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not call `SpeechRecognition.install()`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify. Do not add a silence watchdog. Do not pass a mic track into `rec.start()`.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. No layout diff. No phrase diff. Phrase boost 8 stays inside 0.0–10.0. MDN still warns that 9 or 10 can make other phrases look like Hey Bill.

Live https://bill-morris.netlify.app is still behind that. This run the served hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served script has neither `heldUtterance` nor `releaseMeter`. It still sets `rec.continuous = true` for every browser. Its open line is still two sentences: `` `${you()}. I'm here.` ``. Its who-are-you line is still `Edge is ${lv.edge}.` Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome still must not set `processLocally`. MDN says `start()` after `processLocally = true` throws `language-not-supported` if the en-US pack is not installed. Chromium 542424205 is still an open P2 with no fix: an on-device pack can abort recognition and then fail later sessions until the machine restarts. Leaving the flag unset keeps the cloud path. Do not install a pack.

WebKit bug 326069 is still NEW. Latest comment is still the Radar importer at 2026-10-05 17:30 PDT, linking rdar://problem/189239983. No fix since. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

Main still has no `bill.jpg`, `bill-blink.jpg`, `bill-night.jpg`, or `icon.jpg`. The live deploy has the photo. A publish from a clean clone would drop it. Do not generate a stand-in. Do not upload.

## Still blocking

Netlify free credits still block publish. Live can stay behind main, including the undefined edge line, the two-sentence open, and the old "say Bill" hint. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## Not ready

No READY FOR UPDATE this hour. The only page break found is already on main. The update pass should not edit `index.html`.
