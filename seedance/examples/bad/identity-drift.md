# Bad Example — I2V Identity Drift

A prompt that re-describes the subject when the image is already provided.

> **TL;DR**: paste this to `/seedance fix <prompt>` and the skill will
> remove subject redescription automatically. This file shows the manual
> analysis behind the fix.

## The bad prompt (I2V mode, with anchor image)

```
The same beautiful young east-asian woman in red pajamas with long dark
hair lies on the cream pillow. She has a peaceful expression. The same
woman slowly opens her eyes and looks at the camera. The young woman with
long hair smiles gently. Warm morning light fills the bedroom. The same
girl gently smiles at the camera as light falls on her face.
```

## Lint report

| Rule | Severity | Issue |
|---|---|---|
| R-M01 | 🔴 | I2V prompt re-describes subject multiple times ("the same young east-asian woman", "the same woman", "the young woman with long hair", "the same girl"). Each redescription causes identity drift. |
| R-S02 | 🟡 | Repetitive adjective stacking |
| R-A01 | 🔴 | No `Overall audio atmosphere` |
| R-C01 | 🟡 | No camera move declared |
| R-J03 | 🔴 | No named camera |
| R-N01 | 🔴 | No negatives |
| R-T01 | n/a | No time segments |

**Score: 1/10 — will drift on first generation.**

## What goes wrong

1. **Identity drift across the clip** — the face at 0s vs 2s vs 4s won't
   match. The model "fights" between what the image shows and what the
   prompt re-describes.
2. **Hair becomes hair of a different style** — "long dark hair" repeated
   may conflict with what's in the anchor image
3. **Clothing changes mid-shot** — "red pajamas" may not match anchor
4. **Pacing chaos** — no segments, model invents its own beats
5. **Audio absent** — no BGM

## The fix

The I2V iron rule: **never re-describe the subject**.

```
Natural micro-movement: eyes are barely closed, slowly open as she lifts her gaze toward the bright window, the corner of her mouth lifts into a soft private smile. Subtle breathing rises and falls. Slight handheld shake. Slow push-in toward her face. Warm golden window light catches her cheekbone. Sound: faint fabric rustle, distant birdsong, warm lo-fi piano swells gently. Shot on iPhone 15 Pro, natural skin tone, slight film grain, documentary realism. No distortion, no plastic skin, no waxy face, no oversaturated colors.
```

## What changed

- **Zero subject description** — no "young woman", "east-asian", "long
  hair", "red pajamas". The image carries all of that.
- **Layered motion** — eyes opening → gaze shifting → smile → breathing
  (4 micro-actions stacked)
- **One camera move** — `slow push-in`
- **Named camera** — `iPhone 15 Pro` for UGC look
- **Sound description** with specific physical-sound words
- **4 specific negatives** at the end

**Score: 9/10**

## Memorization key

> "Image carries the subject. Prompt carries the motion."

If you find yourself writing words about who the subject is in an I2V
prompt, delete those words and continue with what they're doing.

## See also

- `../../modes/i2v.md`
- `../good/morning-routine-t2v.md` (where subject DESCRIPTION is correct
  because no image is provided)
