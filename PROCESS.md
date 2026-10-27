# PROCESS.md

## Starting idea

I wanted an experience where the *interaction itself is the artwork* — not a game you operate, but an instrument you play with your body. The image I started from was a record in a dark room: you shout at it, and the grooves throw colour back at you. That gave me the three things the brief asks for immediately: a deliberate action (shout / wave), a meaningful response (sound plus generative visuals), and legible feedback (you can see and hear exactly how loud you were).

The single design rule that shaped everything: **silence and stillness are part of the composition.** If nothing happens when the visitor stops, the piece keeps performing without them. So the band fades out, the sand dies within a second, and the record coasts to a stop. Interaction is not decoration added on top of a loop — it *is* the loop.

## Design / code changes made during development

1. **Energy as a single shared signal.** Voice loudness, camera motion and pointer speed are merged into one smoothed "energy" value (0–1) that drives spin speed, sand emission, spectrum colour, band volume, filter cutoff and tempo. One number keeps every output coherent — when you shout, *everything* leans in together.
2. **Two-dimensional sound mapping.** The first version only used loudness, so every loud moment looked and sounded the same. I added a spectral centroid: brightness of your voice tints the spectrum ring and shifts the palette, so a low hum and a scream at the same volume look different.
3. **Performance rewrite.** The first playable version stuttered badly. The culprits were `shadowBlur` on every particle (the most expensive Canvas operation), full-screen gradients rebuilt every frame, and 46 groove circles redrawn per frame. I rewrote the renderer around pre-rendered glow sprites, offscreen caches for backdrop/disc/vignette/grain, a 900-particle object pool, capped device-pixel-ratio, and adaptive quality that reduces effects when frame times exceed 26 ms. Same look, roughly an order of magnitude cheaper.
4. **Colour direction: cool black.** The first pass had a warm brown room. I moved to a true black environment with a cyan → blue → indigo → violet → magenta palette, so the record reads as an object floating in darkness and the bursts feel like light rather than paint.
5. **Arrangement, not a loop.** Feedback after a first listen was that the music felt thin. The band is now a real arrangement: an Em–C–G–D progression, pad, 16th arpeggio, vibrato lead, tom fills, open hats, ghost snares and a crash every four bars, all through convolution reverb and a damped feedback delay. Layers enter with energy, so the piece grows as you push it.
6. **Graceful degradation.** Camera and microphone are opt-in; denial leaves a working pointer instrument and an explanatory message. Sensor streams are stopped when toggled off, and nothing is uploaded — mention of this is on screen, because a camera-based artwork has to earn trust.

## User trial

*(Fill this in before submitting — ask one person to try it, then replace this paragraph.)*

- **Who tried it:** e.g. a housemate (not identified).
- **What I observed:** e.g. they hesitated at the "CLICK TO BEGIN" curtain because they expected the record to play on its own; once they clicked, they said nothing for a few seconds, then shouted — and laughed at the burst. They did not realise the music stops when they stop until I pointed it out.
- **One improvement I made because of the trial:** e.g. after seeing them wait for the record to start by itself, I made a burst fire as soon as the piece begins, and put the one-line rule ("everything stops the moment you do") in the curtain text and a toast after starting. *(Replace with what actually happened and what you actually changed — or explain why you kept the design.)*

## AI tools used

- **What I used AI for:** drafting the Canvas particle system and the Web Audio instrument definitions, diagnosing the stutter (it identified `shadowBlur` as the bottleneck), and editing this document.
- **One suggestion I kept:** the adaptive-quality idea — measuring frame time and quietly reducing particle counts and effects. It is invisible to the user and it is the reason the piece stays smooth on a laptop.
- **One suggestion I rejected:** the AI proposed pre-selecting the "hit" colour for each burst so every burst would be a tidy two-colour gradient. I rejected it: sampling the palette per grain is what gives the sand its granular, scattering look — tidier was worse.

## Credits

- All code written for this assignment with AI assistance as described above. No libraries, no borrowed code, no audio samples, no images: every sound is synthesized and every visual is drawn at runtime.
- Privacy: camera frames are reduced to a 64×48 buffer and diffed in memory; microphone audio is analysed for level and spectral balance. Neither is stored or transmitted.
