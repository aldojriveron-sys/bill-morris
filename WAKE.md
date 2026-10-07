# Hold the recognizer until the arm-tap prime ends.

Hour: 2026-10-06 23:13 EDT. Research only. Do not re-apply the 22:15 resume. Commit `c9852d9` already calls `speechSynthesis.resume()` and stores the volume-0 space on `heldUtterance` at the top of `arm()`. Do not re-apply the 21:15 silent prime. Do not re-apply the 20:16 desktop hint. Do not re-apply the 19:15 edge-line skip. Do not re-apply the 16:13 timer gate. Do not re-apply the meter release, the wake-gap hold, the apostrophe strip, the 6-character floor, the one-word echo check, the includes check, the sleep lastBill assign, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not call `SpeechRecognition.install()`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify. Do not add a silence watchdog. Do not call `pause()` on the phone. Do not pass a mic track into `rec.start()`. Do not change the wake regex. Do not change the phrase boost. Do not cancel the prime utterance in `arm()`. Do not edit `startRec()`.

## What I learned

The five done checks still hold on main. One tap calls `arm()` and `startRec()`. Hey Bill is the wake path in `hearWake()` plus the phrase bias at 8. `sleep()` sets `awake` false, leaves `armed` true, and starts the recognizer again. The 760px rule is still the phone layout. The open line is still one sentence: `` `${you()}, I'm here.` ``. Phrase boost 8 stays inside 0.0–10.0.

The page break this hour is the race the resume commit left in place. `arm()` queues the volume-0 space, then calls `startRec()` in the same turn, before that utterance can end. Chrome Android shares one audio path between `speechSynthesis` and `SpeechRecognition`. A recognizer started while synthesis is pending often fires `aborted` and then never delivers `onresult`, so Hey Bill is armed and deaf. `resume()` does not close that race. Chrome Android can report `paused` false while the queue is stuck, and `resume()` is a no-op in that state. Do not call `pause()`. A 14-second pause-then-resume watchdog is a different bug and ends the current utterance on Android.

The free unblock is to start listening once, after the prime ends. Attach `onend` and `onerror` on the held prime, and keep a 700ms fallback so a volume-0 space that never fires `onend` does not leave him deaf. A local `opened` flag stops the event and the timer from both calling `startRec()`. `startRec()` already aborts and rebuilds, so this note does not edit it. Do not cancel the prime. `wake()` already sets `heldUtterance` null and `speaking` true before the open line, so a late prime callback must no-op when `speaking` is true.

The desktop hint now matches sleep. It still says the bars are mic gain. `startRec()` still calls `releaseMeter()` before `rec.start()`, on purpose. Do not reopen the meter. Do not edit that hint.

Live https://bill-morris.netlify.app is still behind main. This run the served hint was still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The served page has no prime and no resume. Netlify free credits still block publish. A skipped upload does not publish. Do not upload.

Chrome still must not set `processLocally`. Leaving the flag unset keeps the cloud path. Do not install a pack.

WebKit bug 326069 is still NEW. No resolution. Latest comment is still the Radar importer at 2026-10-05 17:30 PDT, linking rdar://problem/189239983. The bug page still lists no workaround. A new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This note does not claim to fix iOS 27.

Main still has no `bill.jpg`, `bill-blink.jpg`, `bill-night.jpg`, or `icon.jpg`. The live deploy has the photo. A publish from a clean clone would drop it. Do not generate a stand-in. Do not upload.

## Still blocking

Netlify free credits still block publish. Live can stay behind main, including the old "say Bill" hint and the two-sentence open. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open, so a phone can arm once and then go deaf until the tab is closed. There is still no custom Hey Bill model. Do not ship a synthetic one. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only.
Smallest diff: replace the trailing `startRec();` in `arm()` with a one-shot start after the prime. Do not cancel the prime. Do not edit `startRec()`.

```
  let opened = false;
  const hearAfterPrime = () => {
    if (opened || speaking || document.visibilityState === "hidden") return;
    if (!(awake || armed)) return;
    opened = true;
    startRec();
  };
  try {
    if (heldUtterance) {
      heldUtterance.onend = hearAfterPrime;
      heldUtterance.onerror = hearAfterPrime;
    }
  } catch (e) {}
  setTimeout(hearAfterPrime, 700);
```

Do not change the open line, the wake regex, the phrase boost, or the meter.
