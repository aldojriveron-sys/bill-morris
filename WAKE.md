# iPad desktop UA still opens a second mic

Hour: 2026-10-06 08:22 EDT. Research only. Do not re-apply the onend hidden guard. It is already in `startRec` on main (`6c2a50b`, Guard the onend restart against a hidden tab). Do not re-apply the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 07:16 onend note is stale. The update pass already landed that diff. Leave `onend` alone.

The leftover is `phoneMic`. It only matches `Android|iPhone|iPad|iPod` in the user agent. iPadOS 13 and later, in the default desktop site mode, sends a Macintosh UA and does not contain `iPad`. A real iPad then fails `phoneMic()`, so `openMeter` calls `getUserMedia({ audio: true })` and holds a second capture next to `SpeechRecognition`. The phone path already skips that meter and releases it in `startRec` so the recognizer is the only mic owner. iPad misses that. Mac stays at `maxTouchPoints` 0. iPad in desktop mode is typically 5. `Macintosh` plus `maxTouchPoints > 1` is the usual check. Free. No account.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Main already says Hey Bill. Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (last changed 2026-10-05 17:30 PDT). No comments since. No workaround on the bug. This iPad check does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, `phoneMic`:

Replace

`function phoneMic() {
  return /Android|iPhone|iPad|iPod/i.test(navigator.userAgent);
}`

with

`function phoneMic() {
  const ua = navigator.userAgent;
  return /Android|iPhone|iPad|iPod/i.test(ua) || (/Macintosh/i.test(ua) && navigator.maxTouchPoints > 1);
}`

Do not change `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `speak()`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or sleep. Do not add a local model.
