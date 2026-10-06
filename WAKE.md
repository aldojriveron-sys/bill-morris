# No new listening diff. Live desk is still the old arm line.

Hour: 2026-10-06 17:16 EDT. Research only. Do not re-apply the 16:13 timer gate. It is already in `wake()` and `speak()` on main (`heldUtterance = null` after `speaking = true`, and the 320ms timer clears `speaking` only if `heldUtterance === u`). Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. No layout diff. No phrase diff. Phrase boost 8 stays inside 0.0–10.0. MDN still warns that 9 or 10 can make other phrases look like Hey Bill.

Live https://bill-morris.netlify.app is still behind that. At 17:16 the hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served script has neither `heldUtterance` nor `releaseMeter`. Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome's speech service is a singleton. Aborting one recognizer and calling `start()` on the next before that abort finishes can throw InvalidStateError (Chromium voice-search race, fixed in their UI by waiting for the previous end). `startRec()` already catches a failed `start()` and retries at 350ms, then again at 700ms. A forced delay on every restart would widen the gap where Hey Bill is missed. Leave the retry. No diff.

WebKit bug 326069 is still NEW. Last change 2026-10-05 17:30 PDT, last comment the Radar importer, no comments since, no resolution. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

## Still blocking

Netlify free credits still block publish. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## NOT READY

No file change this hour. Do not edit `index.html`. Do not re-apply the 320ms timer gate. The update pass should leave main's listener alone.
