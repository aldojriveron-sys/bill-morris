# Wake yields before the open line holds the mic

Hour: 2026-10-06 14:13 EDT. Research only. Do not re-apply the apostrophe strip, the 6-character floor, or the one-word echo check. They are already in `handle()` on main (`ec8ea281`). Do not re-apply the includes check, the sleep lastBill assign, the 320ms hold, the iPad desktop UA check, the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 13:14 note is stale. The update pass already landed the apostrophe strip, the floor of 6, and `oneWord` inside `handle()`. Leave that guard alone.

The leftover is the gap in `wake()`, before `speak()` sets `speaking`. `wake()` sets `awake = true`, then `await openMeter()`. `await` yields even on a phone, where `openMeter()` returns immediately. `speak()` is what sets `speaking = true`, and it has not run yet. `onresult` only bails when `speaking` is true. After the yield, a final that is not a pure wake phrase passes the command gate and calls `handle()` before `lastBill` is the open line. That happens when the interim alternative was Hey Bill and the top final is a mishear, or when Chrome appends words to the wake phrase. The pure `Hey Bill` final is already blocked by the command gate. This does not cover the other finals.

Hold the session before the yield. Set `speaking = true` and abort the current recognizer in the same turn as `awake = true`, before `await openMeter()`. `speak()` already sets `speaking` and clears it 320ms after the open line. The `onend` and `aborted` restarts already return when `speaking` is true, so the abort does not start a second session under the open line. No new account, no model, no longer mute. Free. Safe.

Phrase boost 8 is inside the 0.0–10.0 range. No phrase diff. MDN still warns that 9 or 10 can make other phrases look like Hey Bill. Leave the bias at 8.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Main already says Hey Bill. Phone layout on main was already clear at 390. No layout diff. Sleep already returns to the wake word. The open line is still one sentence (`Aldo, I'm here.`).

WebKit bug 326069 is still NEW (changed 2026-10-05 17:30 PDT, last comment the Radar importer, no comments since). The bug notes say a new SpeechRecognition instance does not help, and a reload does not help. Only the first session in the tab hears. This wake-gap hold does not claim to fix iOS 27.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `wake()`, the three lines before `openMeter()`:

Replace

`awake = true;
  wakeBtn.textContent = "Bill is live";
  wakeBtn.classList.add("off");
  await openMeter();`

with

`awake = true;
  speaking = true;
  try { rec && rec.abort(); } catch (e) {}
  wakeBtn.textContent = "Bill is live";
  wakeBtn.classList.add("off");
  await openMeter();`

Do not change `handle()`, the sleep lastBill assign, `speak()`, the 320ms timer, `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or `sleep()`. Do not add a local model.
