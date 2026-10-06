# Hey Bill phrase bias is ready

Hour: 2026-10-06 00:13 EDT. Research only. Update pass has not applied this.

## What I learned

Main already does the finished-desk path in `index.html`: one tap calls `arm()`, Chrome `SpeechRecognition` stays up, `hearWake` matches Hey Bill, `sleep()` keeps `armed` and returns to the wake word, the open line is one sentence (`${you()}, I'm here.`), and the phone rule is the existing `@media (max-width: 760px)` block. Do not rebuild that.

The free gap is recognition, not a new model. Chrome's Web Speech API defaults to one hypothesis. Desktop Chrome 142+ can bias that hypothesis with `SpeechRecognitionPhrase` and no account. Can I use: constructor supported in Chrome and Edge 142+, not in Chrome for Android, not in Safari. The WebAudio explainer says Chrome may only apply phrases on-device, and a miss fires `phrases-not-supported`. Forcing `processLocally = true` is not safe: it can drop the cloud line this desk already uses.

So the smallest safe change is optional bias only. If the constructor is missing, or it throws, or the session errors `phrases-not-supported`, set a flag and restart the same recognizer with no phrases. Do not set `processLocally`. Do not change `hearWake`, `speak()`, the open line, the phone rule, sleep, or the audio-capture hint from `5cbfd0d`.

This is not the synthetic Hey Bill draft. Do not add an openWakeWord model, a Porcupine `.ppn`, neon, khaki, or a new interruption.

Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still NEW as of this hour: iOS 27 SpeechRecognition only hears the first session in a tab, and the bug has no page workaround. The brief repo `aldojjriveron02/bill-morris` still 404s. The note lives on `aldojriveron-sys/bill-morris` main. Live page is still https://bill-morris.netlify.app. This pass did not upload to Netlify. Free credits still block publish, so the live desk can stay behind main.

## READY FOR UPDATE

File: `index.html` only.

Add next to the other recognizer flags (`let armed = false;` is fine):

```js
let phraseBiasOff = false;
```

In `startRec`, after `rec.continuous = true;`:

```js
  if (!phraseBiasOff && window.SpeechRecognitionPhrase) {
    try { rec.phrases = [new SpeechRecognitionPhrase("Hey Bill", 8)]; }
    catch (e) { phraseBiasOff = true; }
  }
```

In `rec.onerror`, before the generic fallthrough:

```js
    if (ev.error === "phrases-not-supported") {
      phraseBiasOff = true;
      setTimeout(() => {
        if (ended || rec !== mine || speaking || document.visibilityState === "hidden") return;
        if (!(awake || armed)) return;
        startRec();
      }, 400);
      return;
    }
```

Nothing else.
