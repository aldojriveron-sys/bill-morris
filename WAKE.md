# No-speech restart still ignores a hidden tab

Hour: 2026-10-06 06:14 EDT. Research only. Do not re-apply the hide abort. It is already in the `visibilitychange` listener on main (`e1d91a3`). Do not re-apply the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 05:13 hide abort is already on main. Leaving the tab calls `rec.abort()`, and coming back calls `startRec()`. Leave that listener alone.

The leftover is the `no-speech` restart inside `startRec`. Every other error path (`aborted`, `audio-capture`, `phrases-not-supported`, and the generic restart) bails if `document.visibilityState === "hidden"`. The `no-speech` timer does not. Android Chrome reports a cut session as `no-speech` more often than `aborted`. That timer can then call `startRec` while the tab is still hidden. `startRec` returns on the hidden guard, so the restart is dropped, and a late timer can also land on the session the visible path just started. The same hidden check the other paths already use closes it. Free. No account.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (last changed 2026-10-05 17:30 PDT). No workaround on the bug. This guard does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, the `no-speech` branch in `startRec`:

Replace

`if (ev.error === "no-speech") {
      setTimeout(() => {
        if (!ended && rec === mine && (awake || armed) && !speaking) startRec();
      }, 400);
      return;
    }`

with

`if (ev.error === "no-speech") {
      setTimeout(() => {
        if (ended || rec !== mine || speaking || document.visibilityState === "hidden") return;
        if (!(awake || armed)) return;
        startRec();
      }, 400);
      return;
    }`

Do not change `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `speak()`, the open line, the phone rule, the hide abort, or sleep. Do not add a local model.
