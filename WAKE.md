# Sleep line never enters the echo guard

Hour: 2026-10-06 11:17 EDT. Research only. Do not re-apply the endsWith check. It is already in `handle()` on main (`6b7de54`, Drop a Bill tail that is the end of lastBill). Do not re-apply the 320ms hold, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 10:20 note is stale. The update pass already landed the suffix check. Leave that condition alone.

The leftover is the sleep branch. It speaks `Lights out. Gain stays on the board.` and returns before `lastBill = answer`. `lastBill` stays the previous line, often `Aldo, I'm here.` The endsWith check never sees the sleep line. After the 320ms hold, a tail such as `stays on the board` can still be handed to the desk as a command. Set `lastBill` to that spoken line before `speak` so the guard already on main can drop the tail. No new account, no model, no longer mute. Free. Safe.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." The live script has no 320ms hold, no endsWith check, and no phrase bias. Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (last changed 2026-10-05 17:30 PDT). Three comments, none since. No workaround on the bug. This lastBill assign does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `handle()`, the `__SLEEP__` branch:

Replace

`sayLine("Bill", "Lights out. Gain stays on the board.");
    await speak("Lights out. Gain stays on the board.");`

with

`const line = "Lights out. Gain stays on the board.";
    sayLine("Bill", line);
    lastBill = line;
    await speak(line);`

Do not change the echo condition, `speak()`, the 320ms timer, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or `sleep()`. Do not add a local model.
