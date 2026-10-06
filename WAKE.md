# Hide releases the phone mic

Hour: 2026-10-06 05:13 EDT. Research only. Do not re-apply the heybill command gate. It is already in `handle(finalSaid)`. Do not re-apply the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 04:13 gate is already on main. `index.html` now drops a final that is only the wake token, including collapsed `heybill`. Leave that line alone.

The leftover is the phone mic when the tab is hidden. `startRec` already returns if `document.visibilityState === "hidden"`, and the visible path already calls `startRec`. Nothing aborts the session that is already running. Chrome and Safari keep that speech session in the background. On a phone that holds the mic, so the next visible `start()` is the one that hits `audio-capture` or a silent session. Aborting on hide is free and does not need an account. The existing hidden guard already stops the abort's restart from looping while the tab is away.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The published page has no `heybill`, no `maxAlternatives`, no phrase bias, and `continuous` still forced true. Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (last changed 2026-10-05). No workaround on the bug. This hide abort does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, the `visibilitychange` listener:

Replace

`document.addEventListener("visibilitychange", () => {
  if (document.visibilityState !== "visible") return;
  if ((awake || armed) && !speaking) startRec();
});`

with

`document.addEventListener("visibilitychange", () => {
  if (document.visibilityState !== "visible") {
    try { rec && rec.abort(); } catch (e) {}
    return;
  }
  if ((awake || armed) && !speaking) startRec();
});`

Do not change `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `speak()`, the open line, the phone rule, or sleep. Do not add a local model.
