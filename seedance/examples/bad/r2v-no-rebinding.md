# Bad Example — R2V Without `@image` Rebinding

A prompt that binds `@image1` only in the first segment, then uses
pronouns. Classic failure mode.

> **TL;DR**: paste this to `/seedance fix <prompt>` and the skill will
> rebind `@image` in every segment automatically. This file shows the
> manual analysis behind the fix.

## The bad prompt (R2V mode)

```
Vertical video 9:16. Overall audio atmosphere: kawaii synth with marimba.

[0-5s] @image1 sits on the wooden floor as hands fit the harness onto her. The cat looks up curiously.

[5-11s] She walks down the street, looking at the cow statues. She stops, looks around, then continues walking.

[11-15s] The harness is held up to the camera. The fingers tilt the fabric to show the texture.

Shot on iPhone 15 Pro. No distortion, no plastic skin, no on-screen text.
```

## Lint report

| Rule | Severity | Issue |
|---|---|---|
| R-R01 | 🔴 | `@image1` binding present only in segment 1. Segments 2 and 3 use pronouns ("she", "the harness", "the fingers") with no `@` anchor. |
| R-R03 | 🟡 | Pronoun "she" in multi-asset context not glued to `@image1` |
| R-S01 | 🔴 | Each scene lacks color + material + light triplet |
| R-C01 | 🟢 | One camera move per segment (none stacked — but also none declared) |
| R-C02 | 🟡 | No explicit camera move in segments 2-3 |
| R-J03 | 🟢 | Named camera (iPhone 15 Pro) — barely |
| R-N01 | 🟢 | 3 negatives — within range |

**Score: 5/10 — usable but will face-swap mid-shot.**

## What goes wrong

1. **Segment 2 cat may not be the same cat** as in `@image1`. Model
   generates a different cat ("she walks down the street") because there's
   no anchor.
2. **Segment 3 harness may not match** the harness from `@image1` /
   `@image4`. Color or style drift.
3. **Scene jumps unpredictably** — no `@image2` or `@image3` to anchor the
   indoor / outdoor / product-detail contexts.
4. **Scene description sparse** — "the street" gives no color, material,
   or light. Model invents random street.

## The fix

```
Vertical video 9:16. Overall audio atmosphere: cheerful kawaii synth music at 115 BPM with marimba and pizzicato strings — match @audio1.

[0-5s] @image1 = white British Shorthair silver shaded, wearing a pink mesh harness. @image2 = indoor scene (light wood-grain floor, white door frame, warm window light). Close-up handheld shot. Two human hands fit the harness onto @image1, click the buckle, attach the leash. @image1 sits patient on the floor. Sound: fabric rustle, soft plastic click.

[5-11s] @image1 wears the same harness, now in @image3 = outdoor scene (gray sidewalk, red brick planter, black-and-white cow statues, yellow wildflowers). Medium handheld follow, low-angle. @image1 walks forward, stops, looks up at the cow statues curiously, then continues. Sound: pizzicato bounces, ambient street air.

[11-15s] Cut to extreme close-up of @image4 = pink mesh harness held in a hand. Fingers tilt the fabric to show breathable grid texture and reflective trim. Very shallow depth of field, blurred greenery behind. Locked handheld wobble. Sound: marimba final note.

Shot on iPhone 15 Pro. Natural color, slight handheld bobbing, warm color grading, documentary UGC realism. Follow the cut pacing of @video1, BGM energy of @audio1.

Constraints: face stable, no distortion, motion fluid, no plastic skin, no oversaturated colors, no on-screen text.
```

## What changed

- **`@image1` re-bound in every segment** — segment 1, 2, AND 3 mention
  `@image1` (not "she")
- **`@image_N` for each scene** — `@image2` (indoor), `@image3` (outdoor),
  `@image4` (product detail)
- **Scene triplets** added: color + material + light for each
- **One camera move per segment** explicit: handheld → handheld follow →
  locked wobble
- **Asset role annotation** in segment 1 (`@image1 = white British
  Shorthair`)
- **Pacing reference** explicit (`Follow the cut pacing of @video1`)

**Score: 9/10**

## Memorization key

> "Rebind every segment. The model has no memory between segments."

If you typed an `@image` once and then a pronoun later, your binding is
gone. Treat every segment as fresh.

## See also

- `../../modes/r2v.md` (Iron rule #1)
- `../good/pet-vlog-r2v.md`
