# Bad Example — Junk Token Soup (T2V)

A prompt that LOOKS impressive but actively HURTS output quality.

> **TL;DR**: paste this prompt to `/seedance fix <prompt>` and you'll get
> the fixed version + diff explanation in one shot. This file shows the
> manual analysis behind that fix.
> See [the headline FIX demo](../good/fix-junk-prompt.md) for the
> auto-rewrite case study.

## The bad prompt

```
A beautiful young woman waking up in her cozy bedroom in the morning,
she stretches and smiles, warm sunlight pouring through the window,
cinematic, highly detailed, 8k, beautiful composition, masterpiece,
amazing quality, breathtaking, stunning, ultra-realistic, perfect
lighting, award-winning photography.
```

## Lint report

| Rule | Severity | Issue |
|---|---|---|
| R-J01 | 🔴 | Multiple junk tokens: `cinematic` (standalone), `highly detailed`, `8k`, `masterpiece`, `amazing quality`, `breathtaking`, `stunning`, `ultra-realistic`, `award-winning` |
| R-J02 | 🟡 | Adjective chains: "beautiful, breathtaking, stunning..." |
| R-J03 | 🔴 | Missing named camera — model gets no anchor for actual aesthetic |
| R-S01 | 🔴 | No color + material + light triplet — "cozy bedroom" + "warm sunlight" is two of three at best |
| R-S02 | 🟡 | Vague subject: "beautiful young woman" — no age, no hair, no clothing, no feature |
| R-C01 | 🟡 | No segment structure — single long sentence |
| R-C02 | 🟡 | No camera move declared |
| R-A01 | 🔴 | No `Overall audio atmosphere` declaration |
| R-N01 | n/a | No negatives at all — also bad (different problem) |
| R-T01 | n/a | No time segments — pacing entirely up to model |

**Score: 1/10 — fundamentally broken.**

## What goes wrong in the output

1. Generic AI-pretty face (no identity anchor)
2. Generic bedroom (no specific palette / material)
3. Generic "warm" light (no direction, no time-of-day signal)
4. Random camera behavior (no move declared)
5. AI-aesthetic over-polished look (junk tokens trigger this)
6. Plasticky skin, glassy eyes, overdone smile
7. No audio coherence
8. Pacing unclear — model may run out of "stuff" by 5s

## The fix

```
Vertical video 9:16. Overall audio atmosphere: warm lo-fi piano with gentle female humming, soft morning ambience with distant birdsong, no voiceover.

[0-3s] Extreme close-up on a cream linen pillow, soft golden sunlight from camera-right, slight handheld micro-shake. A young east-asian woman with shoulder-length dark hair, cream linen pajamas, her hand slowly drifts into frame and brushes a strand of hair off her cheek. Beige walls, cream linen sheets, warm wood floor. Sound: faint fabric rustle, distant birdsong.

[3-7s] Cut to medium shot, low angle from the bedside. She lifts her head off the pillow, eyes barely open, exhales slowly, then pushes herself up with one elbow. A single ceramic mug on the nightstand catches a rim of light. Camera holds still. Sound: bedding swoosh, mug clinks softly.

[7-10s] Cut to close-up on her face from the side. She tilts her head toward the window, eyes squint at the brightness, the corner of her mouth lifts into a small private smile. Warm sunlight crosses her cheekbone. Sound: lo-fi piano swells gently.

Shot on Sony A7S3, natural skin tone, slight film grain, documentary realism. Face stable, no distortion, body proportions natural, motion fluid, no flicker, no plastic skin, no oversaturated colors, no on-screen text.
```

## What changed

- Removed all 9 junk tokens (`cinematic`, `highly detailed`, `8k`, etc.)
- Added named camera (`Sony A7S3`)
- Added 3 time segments with explicit motion sequencing
- Added scene triplets per segment (color + material + light)
- Added subject anchor (age + hair + clothing)
- Added Overall audio atmosphere
- Added 5 specific negatives at the end
- Added one camera move per segment (handheld → locked → locked)

**Score: 9/10**
