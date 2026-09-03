# GAYDAR 9000

A gag app. It scans a face and a voice with a genuinely real instrument, then
tells absolutely everyone the same thing.

## The design rule

The joke is aimed at the machine, not at the person. The app is a parody of
orientation-"detection" tech — the pseudoscience is the punchline, and the
reveal makes that explicit: everyone gets the same verdict, so the verdict
means nothing. Any change that makes the app read as a genuine verdict on the
user breaks the joke and turns the app into a harassment tool. Don't make that
change.

## The second design rule: the instrument is real, the conclusion is not

The voice analysis is not faked. Pitch, level, voicing, spectral centroid and
syllable count are real signal processing, computed live from the microphone,
and shown to the user in the reveal as their actual numbers.

None of it feeds the verdict. That gap is the entire joke, and it is stated
outright on the reveal screen: *a real instrument and an invented conclusion is
exactly how orientation-"detection" tech works too.* Fake theater over fake
measurement would be a weaker gag and a weaker piece of engineering.

## How it works

`index.html` is the whole app. No build step, no dependencies, no server, no
network calls of any kind.

| Stage | What actually happens |
|---|---|
| Face lock | `FaceDetector` where the platform has it; elsewhere frame-difference stillness calibrated against the camera's own noise floor. Auto-captures on a 3-2-1 countdown once locked. |
| Camera | `getUserMedia` → `<video>` → canvas. Stays in memory. |
| Pitch | Normalized square difference (NSDF) with first-major-peak selection and parabolic interpolation, over a 70–400 Hz lag window. |
| Syllables | Fast/slow envelope followers with a 110 ms refractory gap, run every animation frame. |
| "Analysis" | 4.2 s of animation. Nothing is computed from the image. |
| Result | `84 + (hash % 16)`. The word "GAY" is a string literal. |
| Card | Rendered client-side into a 1080×1350 canvas. |

Measured accuracy on synthesized signals: **pitch within a cent** from 85–330 Hz
(pure and 6-harmonic), correctly reports unvoiced on silence and sub-0.6 clarity
on white noise. On a synthetic 165 Hz voice at 4 syllables/sec it recovers
165 Hz exactly, 13 of 14 syllables, and 3.4 s of a 3.5 s utterance.

The percentage is seeded from the captured frame plus the measured median pitch
and speech duration, so a re-scan of the same person gives the same number. It
feels deterministic, which sells the bit better than fresh randomness.

## Why there is no speech recognition

The obvious way to check the phrase was spoken is the Web Speech API. It is the
wrong choice here for two independent reasons:

1. **`SpeechRecognition` uploads audio to a cloud service by default** (Google's,
   in Chrome). That would silently make the app's "nothing leaves your device"
   promise false. Chrome 139+ offers `processLocally: true`, but it is
   Chrome-only and very new.
2. **It hung the renderer in testing.** `SpeechRecognition.available()` never
   resolved in Chromium 1194 — not a rejected promise, a hard hang that
   `try`/`catch` cannot recover from.

Counting syllable onsets from the energy envelope verifies phrase length, is
real local analysis, works in every browser, and cannot hang. If you ever
reconsider this, keep reason 1 in mind: cloud transcription changes what the
app is, not just how it's built.

## Operator control

Press and hold the wordmark for 1.2 s to cycle which band the percentage lands
in — auto → low (84–87) → high (96–99). Confirmed by haptic and a tone; the
brand dot shifts slightly. This is the standard feature of the prank-scanner
genre: the person running the joke presets the outcome before handing the phone
over. The verdict itself never changes, only the number.

## Privacy

There is no backend. No photo, audio, result, or analytics leaves the device.
Speech is never transcribed. Camera and mic tracks stop on completion and on
`pagehide`.

## Degraded paths (all tested)

Camera denied or absent → photo upload, or scan with no photo.
Stillness never settles → falls back to manual capture after 9 s.
Mic denied or absent → skip; the scan still runs.
Nothing heard within 9 s → offers a retry instead of a fake result.
No Web Share API → falls back to `<a download>`, then to "just screenshot it."

## Run locally

    npx http-server -p 8199 .
    open http://127.0.0.1:8199/gaydar/

`getUserMedia` requires a secure context — `localhost` or HTTPS. Opening the
file over `file://` will fall back to the no-camera path.
