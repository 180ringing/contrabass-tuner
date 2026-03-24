# 🎸 Contrabass Tuner

> **🇷🇺 [Русская версия](README.ru.md)**

A browser-based chromatic tuner optimized for double bass (contrabass). Single HTML file, works in any modern browser, fullscreen — designed to be visible from a distance while practicing.

**[▶ Open Tuner](https://egorandreev.github.io/contrabass-tuner/)** — works directly in your browser, no installation needed.

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

**Option 1: Online** — open the [live version](https://egorandreev.github.io/contrabass-tuner/)

**Option 2: Local** — download `index.html` and open in browser. Requires internet for first load (pitchy library from CDN).

Microphone access required. Works on Chrome, Firefox, Edge (desktop and mobile). Safari supported with user gesture.

## Signal Path

```
Microphone → Lowpass Filter → AnalyserNode
  → [1] Attack Detection (blanking)
  → [2] RMS Gate (adaptive volume threshold)
  → [3] Spectral Flatness Gate (noise vs tone)
  → [4] Pitch Detection (pitchy MPM)
  → [5] Median Filter (kills octave jumps)
  → [6] Octave Lock (snap back ×2/×0.5)
  → [7] Note + Cents conversion
  → [8] Note Change Detection
  → [9] EMA Smoothing
  → Display: Note name, LED bar, Lane canvas
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
