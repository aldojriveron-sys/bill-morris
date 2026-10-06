# End restart still ignores a hidden tab

Hour: 2026-10-06 07:16 EDT. Research only. Do not re-apply the no-speech hidden check. It is already in `startRec` on main (`1388dd8`). Do not re-apply the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 06:14 no-speech guard is already on main. Leave that branch alone.

The leftover is `rec.onend`. Hide abort still fires `end`. That handler sets `ended` and, if the session is still current, schedules a bare `startRec` in 250ms. The check `rec !== mine` runs before the timer, not inside it. Every error path already bails inside its timer if the tab is hidden or the session was replaced. This one does not.

`startRec` returns immediately if the tab is still hidden, so a late timer that fires while hidden is a no-op. The break is the short return. Hide at t=0, come back at t=100, `visibilitychange` starts a new session. The old `onend` timer fires at t=250 and calls `startRec`, which aborts the session the visible path just started. Phone mic flaps. The same hidden check and `rec !== mine` test the error timers already use closes it. Free. No account.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Main already says Hey Bill. Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (last changed 2026-10-05 17:30 PDT). No workaround on the bug. This guard does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, the `onend` handler in `startRec`:

Replace

`rec.onend = () => {
    ended = true;
    if (rec !== mine) return;
    if ((awake || armed) && !speaking) setTimeout(startRec, 250);
  };`

with

`rec.onend = () => {
    ended = true;
    if (rec !== mine) return;
    setTimeout(() => {
      if (rec !== mine || speaking || document.visibilityState === "hidden") return;
      if (!(awake || armed)) return;
      startRec();
    }, 250);
  };`

Do not change `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `speak()`, the open line, the phone rule, the hide abort, the no-speech branch, or sleep. Do not add a local model.
