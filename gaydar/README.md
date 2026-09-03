# GAYDAR 9000

A gag app. It "scans" a face and a voice with entirely fake science and tells
absolutely everyone the same thing, with a random percentage attached.

## The design rule

The joke is aimed at the machine, not at the person. The app is a parody of
orientation-"detection" tech — the pseudoscience is the punchline, and the
reveal makes that explicit: everyone gets the same verdict, so the verdict
means nothing. Any change that makes the app read as a genuine verdict on the
user breaks the joke and turns the app into a harassment tool. Don't make that
change.

## How it works

`index.html` is the whole app. No build step, no dependencies, no server, no
network calls of any kind.

| Stage | What actually happens |
|---|---|
| Camera | `getUserMedia` → `<video>` → drawn to a canvas. Stays in memory. |
| Voice | `getUserMedia` audio → `AnalyserNode`. The waveform is real live audio. Audio is never recorded — only an average amplitude is kept, as a number. |
| "Analysis" | A 4.2s animation. Nothing is computed from the image. |
| Result | `84 + (hash % 16)`. The word "GAY" is a string literal. |
| Card | Rendered client-side into a 1080×1350 canvas. |

The percentage is derived from a hash of the captured frame and the voice
amplitude, so re-running on the same face gives the same number — it feels
deterministic, which sells the bit better than a fresh random each time.

## Privacy

There is no backend. No photo, audio, result, or analytics leaves the device.
Camera and mic tracks are stopped on completion and on `pagehide`.

## Degraded paths (all tested)

Camera denied or absent → photo upload, or scan with no photo.
Mic denied or absent → skip; the scan still runs.
No Web Share API → falls back to `<a download>`, then to "just screenshot it."

## Run locally

    npx http-server -p 8199 .
    open http://127.0.0.1:8199/gaydar/

`getUserMedia` requires a secure context — `localhost` or HTTPS. Opening the
file over `file://` will fall back to the no-camera path.
