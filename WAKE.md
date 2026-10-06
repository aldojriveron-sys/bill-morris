# Tail of Bill's line still passes the echo guard

Hour: 2026-10-06 10:20 EDT. Research only. Do not re-apply the 320ms hold. It is already in `speak()` on main (`4a42842`, Hold the mic 320ms after speech end so the tail is not a command). Do not re-apply the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 09:26 note is stale. The update pass already landed the 320ms delay. Leave that timer alone.

The leftover is the check in `handle()`. It only drops a line that contains the first 22 characters of `lastBill`. The open line is `Aldo, I'm here.` A speaker tail heard as `I'm here` does not contain `Aldo, I'm here`, so after the 320ms it can still be handed to the desk as a command. Sleep has the same hole: `Lights out. Gain stays on the board.` A tail of eight or more characters that is the end of that line, such as `stays on the board`, also misses the prefix. Chrome's `speechSynthesis` `onend` is still early; 320ms does not cover a slow speaker or a Bluetooth lag. A content check does not need a new account, a model, or a longer mute. Free. Safe.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The live script has no 320ms hold, no iPad desktop mic check, and no phrase bias. Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence.

WebKit bug 326069 is still NEW (last changed 2026-10-05 17:30 PDT). No comments since. No workaround on the bug. This tail check does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `handle()`, the echo return:

Replace

`if (!clean || (lastBill && clean.toLowerCase().includes(lastBill.toLowerCase().slice(0, 22)))) return;`

with

`const said = clean.toLowerCase().replace(/[^a-z0-9' ]/g, " ").replace(/\s+/g, " ").trim();
  const bill = (lastBill || "").toLowerCase().replace(/[^a-z0-9' ]/g, " ").replace(/\s+/g, " ").trim();
  if (!clean || (bill && (said.includes(bill.slice(0, 22)) || (said.length >= 8 && bill.endsWith(said))))) return;`

Do not change `speak()`, the 320ms timer, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or sleep. Do not add a local model.
