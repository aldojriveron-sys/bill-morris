# Middle of Bill's line still passes the echo guard

Hour: 2026-10-06 12:15 EDT. Research only. Do not re-apply the sleep lastBill assign. It is already in `handle()` on main (`83d503d`, Set lastBill before the sleep line so the echo guard can drop the tail). Do not re-apply the endsWith check, the 320ms hold, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 11:17 note is stale. The update pass already landed `lastBill = line` before `speak` on the sleep branch. Leave that assign alone.

The leftover is the match itself. The guard is still `said.includes(bill.slice(0, 22))` or `said.length >= 8 && bill.endsWith(said)`. A tail that is the end of the line is caught. A tail from the middle is not. After the sleep line, `gain stays on` is 13 characters, `lights out gain stays on the board` does not end with it, and it does not contain the first 22 characters. Same hole on the open line for a middle slice of `Aldo, I'm here.` Chrome's `speechSynthesis` `onend` is still early; the 320ms hold does not cover a slow speaker. Widening `endsWith` to `includes` uses the guard already on main. No new account, no model, no longer mute. Free. Safe. A follow-up that is eight or more characters and happens to sit inside the previous answer will also drop. That is the same class of risk as the suffix check already shipped.

Phrase boost 8 is inside the 0.0–10.0 range. No phrase diff. MDN still warns that 9 or 10 can make other phrases look like Hey Bill. Leave the bias at 8.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The live script has no Hey Bill, no 320ms hold, no endsWith check, and no phrase bias. Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (changed 2026-10-05, last comment 2026-10-05 17:30 PDT, Radar importer). No comments since. The bug notes say a new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This includes check does not claim to fix iOS 27.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `handle()`, the echo return:

Replace

`if (!clean || (bill && (said.includes(bill.slice(0, 22)) || (said.length >= 8 && bill.endsWith(said))))) return;`

with

`if (!clean || (bill && (said.includes(bill.slice(0, 22)) || (said.length >= 8 && bill.includes(said))))) return;`

Do not change the sleep lastBill assign, `speak()`, the 320ms timer, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or `sleep()`. Do not add a local model.
