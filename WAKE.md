# Speech end restarts the mic while the voice is still in the air

Hour: 2026-10-06 09:26 EDT. Research only. Do not re-apply the iPad desktop UA check. It is already in `phoneMic` on main (`a4a724b`, Treat iPad desktop UA as a phone mic). Do not re-apply the onend hidden guard, the no-speech hidden check, the hide abort, the heybill command gate, the Android continuous flag, the hearWake heybill match, the alternatives scan, or the phrase bias. Do not set `processLocally`. Do not ship a synthetic Hey Bill model. Do not paste a Picovoice key. Do not upload to Netlify.

## What I learned

The 08:22 iPad note is stale. The update pass already landed that diff. Leave `phoneMic` alone.

The leftover is the handoff after `speak()`. `done()` sets `speaking` false and resolves on `onend`. `wake()` and `handle()` then call `startRec()` on the next line. Chrome often fires `speechSynthesis` `onend` before the speakers are quiet. The echo guard only drops a line that contains the first 22 characters of `lastBill`. The open line is `Aldo, I'm here.` A tail heard as `I'm here` does not contain that prefix, so it can be handed to the desk as a command. Sleep has the same hole after `Lights out. Gain stays on the board.` Free. No account. No new model.

Live https://bill-morris.netlify.app is still behind main. Its hint is still "One tap arms the mic. Then say Bill. Say sleep to drop back to the wake word." Main already says Hey Bill. Phone layout on main was already clear at 390×844. No layout diff. Sleep already returns to the wake word. The open line is still one sentence.

WebKit bug 326069 is still NEW (last changed 2026-10-05 17:30 PDT). No comments since. No workaround on the bug. This tail wait does not claim to fix iOS 27 hearing only the first session.

## Still blocking

Netlify free credits still block publish. Do not upload. Live can stay behind main. Picovoice still needs a pasted key. Do not paste one. WebKit bug 326069 is still open. The brief repo `aldojjriveron02/bill-morris` still 404s. This note lives on `aldojriveron-sys/bill-morris` main.

## READY FOR UPDATE

File: `index.html` only. Smallest diff, inside `speak()`, the `done` callback:

Replace

`const done = () => {
      if (settled) return;
      settled = true;
      clearTimeout(watch);
      speaking = false;
      if (heldUtterance === u) heldUtterance = null;
      resolve();
    };`

with

`const done = () => {
      if (settled) return;
      settled = true;
      clearTimeout(watch);
      setTimeout(() => {
        speaking = false;
        if (heldUtterance === u) heldUtterance = null;
        resolve();
      }, 320);
    };`

Do not change `hearWake`, the command gate, the phrase-bias block, `maxAlternatives`, `continuous`, `phoneMic`, the open line, the phone layout rule, the hide abort, the no-speech branch, `onend`, or sleep. Do not add a local model.
