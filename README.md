# 🎸 Contrabass Tuner

> **[RU](README.ru.md)**

A browser-based chromatic tuner optimized for double bass (contrabass). Single HTML file, works in any modern browser, fullscreen — designed to be visible from a distance while practicing.

**[▶ Open Tuner](https://180ringing.github.io/contrabass-tuner/)** — works directly in your browser, no installation needed.

![Contrabass Tuner Screenshot](screenshot.png)

## Why

I play bass guitar and I'm learning contrabass. On a fretless instrument there are no landmarks — you have to learn intonation by ear. Regular tuners show only the current deviation, but I wanted to see the *history* — how steadily I hold a note and what happens during transitions. Then I got carried away and built the tuner of my dreams.

A web page was the simplest solution: one file, open in browser, fullscreen on a laptop, visible from across the room while standing at the contrabass.

## Features

- **Lane display** — real-time pitch trail. Green corridor = in tune. Shows intonation stability over the last few seconds
- **LED bar** — instant deviation indicator, like a hardware tuner
- **Attack detection** — blanks the noisy transient on pizzicato pluck
- **Octave lock** — prevents G1↔G2 jumps caused by strong harmonics, unlocks on new pluck
- **Lowpass filter** — weakens upper harmonics before analysis, helping the detector find the fundamental
- **Spectral flatness** — distinguishes notes from noise by spectrum shape
- **Adaptive RMS gate** — rejects quiet background noise but follows the decaying tail of a note
- **Bilingual UI** — English and Russian, auto-detected from browser locale
- **Settings persistence** — all settings saved to localStorage

## Quick Start

**Option 1: Online** — open the [live version](https://180ringing.github.io/contrabass-tuner/)

**Option 2: Local** — download `index.html` and open in browser. Requires internet for first load (pitchy library from CDN).

Microphone access required. Works on Chrome, Firefox, Edge (desktop and mobile). Safari supported with user gesture.

## Signal Path

```
Microphone
  │
  ▼
Lowpass Filter ─────────── lowpassFreq: 350 Hz
  │                        Weakens upper harmonics
  ▼
AnalyserNode ──────────── bufferSize: 4096 (~93 ms)
  │
  ├─ waveform (inputBuffer)
  │    │
  │    ▼
  │  [1] Attack Detection ─ attackRmsRatio: 3.0
  │    │                     attackBlankMs: 120
  │    │                     + octave lock reset
  │    ▼
  │  [2] RMS Gate ────────── rmsGateHigh / rmsGateLow
  │    │                     rmsGateRelease: 0.9985
  │    ▼
  ├─ spectrum (freqBuffer)
  │    │
  │    ▼
  │  [3] Spectral Gate ──── spectralThreshold: 0.25
  │    │                     0=tone, 1=noise
  │    ▼
  │  [4] Pitch Detection ── clarityThreshold: 0.92
  │    │  (pitchy MPM)       minFrequency / maxFrequency
  │    ▼
  │  [5] Median Filter ──── medianWindow: 7
  │    │  (on Hz)
  │    ▼
  │  [6] Octave Lock ────── octaveLockEnabled
  │    │  snap ×2/×0.5
  │    ▼
  │  [7] Freq → Note+Cents
  │    │
  │    ▼
  │  [8] Note Change Detection
  │    │
  │    ▼
  │  [9] EMA Filter ─────── smoothingAlpha: 0.15
  │    │  (on cents)
  │    ▼
  ├────┼────────────┐
  ▼    ▼            ▼
Note  LED bar     Lane (canvas)
±¢    21 seg.     trail + corridor
▸ ◂   + mark      greenZoneCents: 13
                  trailDurationSec: 3
```

## Settings

Main settings are exposed as sliders. Fine-tuning available in the engineering panel (click "Engineering parameters" in settings).

All settings are saved automatically and persist across sessions.

## Tech Stack

- Vanilla HTML/CSS/JS — single file, no build step
- [pitchy](https://github.com/ianprime0509/pitchy) — McLeod Pitch Method for pitch detection
- Web Audio API — microphone input and audio processing
- Canvas 2D — lane rendering

## Notes

- Tested on MacBook with built-in microphone
- Built primarily for contrabass — thresholds tuned for its timbre and low frequencies
- Works well on bass guitar. Might work for guitar too
- Built with [Claude Code](https://claude.ai/code)

## License

[MIT](LICENSE) — do whatever you want with it.

## Contact

- Telegram: [@egorandreev](https://t.me/egorandreev)
- Music & tech channel: [@grooveops](https://t.me/grooveops)
