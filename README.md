# 🕐 Emoji Clock

A single-page web app that tells the time using nothing but clock emoji — rotated to actually point at the right hour and minute.

There are 24 clock emoji in Unicode (12 for each hour, 12 for half-past). Each one has a fixed angle between its hour and minute hands. This app finds the emoji whose hand-angle best matches the current time, then CSS-rotates it so the hour hand points where it should. The minute hand follows for free since the angle between hands was already matched.

**[Try it live →](https://kealjones.com/emoji-clock)**

## How It Works

1. **Angle matching** — For the current time, calculate the angle between the real hour and minute hands. Compare to all 24 emoji and pick the closest match.
1. **Rotation** — Rotate the chosen emoji so its hour hand aligns with the real hour-hand position. Since the angle between hands was matched in step 1, the minute hand lands in approximately the right spot too.
1. **Center calibration** — At startup, each emoji is rendered to an offscreen canvas to measure its actual visual center (bounding box). A CSS `translate()` correction keeps the clock face centered as emojis swap.
1. **Crossfade** — Two stacked text layers alternate, crossfading on swap to smooth out transitions between different emoji glyphs.

The worst-case error is about 7.5° on the minute hand, since the emoji only come in 30-minute increments.

## Features

- ⏱️ **Speed simulation** — Run time at 1×, 10×, 60×, 360×, or 3600× to watch the emoji cycle
- 🥈 **Runner-ups** — See the 2nd and 3rd closest emoji matches, also rotated
- 📊 **Stats** — Live display of hour hand angle, minute hand angle, angle between hands, and error
- 📐 **Sorted grid** — All 24 emoji sorted by their internal hand-angle, with a needle showing where the current time falls
- 🔴 **Center dot** — Tap the clock to toggle a red dot showing the measured rotation pivot
- 📱 **Works everywhere** — Single HTML file, no dependencies, uses your system’s native emoji

## Usage

Just open `emoji-clock.html` in any browser. That’s it — it’s a single self-contained file.

## The iOS Rabbit Hole

A surprising amount of this project went into dealing with Apple’s 3D clock emoji on iOS. Each glyph has subtle artistic variations in hand positioning, asymmetric drop shadows, and 3D perspective effects that shift the visual center. We tried:

- Canvas pixel scanning to detect actual hand angles (canvas renders emoji differently than the DOM on iOS — flat vs 3D)
- Dual-ring detection, tip-distance profiling, direct correction search
- Bright-pixel center detection (failed because the 3D gradient shifts bright pixels upward)

In the end, the simplest approach won: bounding-box center measurement + translate correction + crossfade. Sometimes the best code is the code you delete.

## Credits

Imagined by Greg Littlefield and Executed by **[Keal Jones](https://github.com/kealjones)**. Built with **[Claude](https://claude.ai)** by Anthropic — the entire app was developed through conversation, including all the iterative debugging with real device screenshots.

## License

MIT
