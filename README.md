# Greg Janik — WKHR 91.5

A static web app collecting Greg Janik's on-air talking parts, trimmed from off-air
recordings of WKHR 91.5 (Bainbridge, OH). Same engine as the David app (wkhrdavid.jbf.com):
a synced-stack player with karaoke transcripts. No build step, no framework.

```
index.html      the app (HTML + CSS + JS)
data.js         all clip data: transcripts, timings, slots, audio paths
audio/*.mp3     one trimmed spoken clip per entry
```

`data.js` sets `window.WKHR`, loaded as a plain script, so the page works by
double-clicking `index.html` locally and when served on the web.

**Status:** scaffold live at [gregjanik.jbf.com](https://gregjanik.jbf.com). `data.js` is
empty until Greg's clips are trimmed and added — the app shows a "Clips Coming Soon"
placeholder while `order` is empty.

## Add a clip

1. Trim Greg's spoken segment to mono 32k MP3 into `audio/`, e.g. `audio/sep07.mp3`:
   ```bash
   ffmpeg -ss <start_sec> -i raw.m4a -t <dur_sec> -ac 1 -b:a 32k audio/sep07.mp3
   ```
2. Add one entry to `window.WKHR` in `data.js`: add the key to `order` and add its
   object under `weeks` (`label`, `sub`, `captured`, `slots`, `lengthMin`,
   `lines` = `[seconds, text, [slotKeys]]`, `audio`). Note: the `slots`/`lines`
   schema was built for David's sunrise liturgy; adapt it to whatever repeats in
   Greg's segments.
3. Commit and push — Vercel auto-deploys.

## Test

```bash
node test/sync.test.js   # verifies the synced-stack alignment once clips exist
```

Built for Dragon. Not a Red Cross product.
