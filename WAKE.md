# A short tail of the open line still passes the echo guard

Hour: 2026-10-06 13:14 EDT. Research only. Do not re-apply the includes check. It is already in `handle()` on main (`4a121dd`, Catch a middle echo of Bill's line in the guard). Do not re-apply the sleep lastBill assign, the 320ms hold, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 12:15 note is stale. The update pass already landed `bill.includes(said)` for a slice of 8 or more characters. Leave that includes alone.

The leftover is shorter than that floor, and the apostrophe. The open line is `Aldo, I'm here.` The guard keeps the apostrophe, so `bill` is `aldo i'm here`. Chrome often returns the tail as `I'm here` or `Im here`. With the apostrophe, `i'm here` is 8 characters and is caught. Without it, `im here` is 7 characters, `bill.includes` is false, and the 8-character floor drops it through. A one-word tail is worse: `here` is 4 characters, `board` after `Lights out. Gain stays on the board.` is 5. Neither contains the first 22 characters. The 320ms hold does not cover a slow speaker. Strip apostrophes before the compare, lower the includes floor from 8 to 6 so `im here` matches, and drop a single word of 4 or more letters only when it is a whole word in `lastBill`. `stay` does not match `stays`. No new account, no model, no longer mute. Free. Safe. A 6-character follow-up that sits inside the previous answer will also drop. That is the same class of risk as the includes check already shipped. A one-word command that is a whole word in the previous answer will also drop.

Phrase boost 8 is inside the 0.0–10.0 range. No phrase diff. MDN still warns that 9 or 10 can make other phrases look like Hey Bill. Leave the bias at 8.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The live script has no Hey Bill, no 320ms hold, no includes check, and no phrase bias. Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (changed 2026-10-05 17:30 PDT, last comment the same minute, Radar importer). No comments since. The bug notes say a new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This short-tail check does not claim to fix iOS 27.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `handle()`, the two normalizer lines and the echo return:

Replace

`const said = clean.toLowerCase().replace(/[^a-z0-9' ]/g, " ").replace(/\s+/g, " ").trim();
  const bill = (lastBill || "").toLowerCase().replace(/[^a-z0-9' ]/g, " ").replace(/\s+/g, " ").trim();
  if (!clean || (bill && (said.includes(bill.slice(0, 22)) || (said.length >= 8 && bill.includes(said))))) return;`

with

`const said = clean.toLowerCase().replace(/'/g, "").replace(/[^a-z0-9 ]/g, " ").replace(/\s+/g, " ").trim();
  const bill = (lastBill || "").toLowerCase().replace(/'/g, "").replace(/[^a-z0-9 ]/g, " ").replace(/\s+/g, " ").trim();
  const oneWord = said.length >= 4 && !said.includes(" ") && (" " + bill + " ").includes(" " + said + " ");
  if (!clean || (bill && (said.includes(bill.slice(0, 22)) || (said.length >= 6 && bill.includes(said)) || oneWord))) return;`

Do not change the sleep lastBill assign, `speak()`, the 320ms timer, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or `sleep()`. Do not add a local model.
