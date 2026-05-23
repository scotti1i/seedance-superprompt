# Case study: fixing a junk Seedance prompt

This example shows what the `FIX` workflow is designed to do: remove vague AI-image tokens, add temporal structure, and make the prompt paste-ready for Seedance clients.

## Input

```text
A beautiful young woman drinking coffee in a cozy cafe, cinematic,
highly detailed, masterpiece, 8k, amazing quality
```

## Lint result

Score: **1/10**

| Rule | Problem |
|---|---|
| R-J01 | Junk tokens: `cinematic`, `highly detailed`, `masterpiece`, `8k` |
| R-S01 | Missing color + material + light scene triplet |
| R-T01 | No time segments, so pacing will be random |
| R-A01 | Missing `Overall audio atmosphere` |
| R-C02 | No camera movement |
| R-J03 | No named camera |
| R-N01 | No specific negative constraints |

## Fixed prompt

```text
Vertical video 9:16. Overall audio atmosphere: warm lo-fi piano with
soft espresso machine hiss, distant cafe chatter, no voiceover.

[0-3s] Medium close-up handheld shot. A young east-asian woman with
shoulder-length dark hair, cream cashmere sweater, sits at a marble
counter. Warm tungsten pendant light overhead, dark walnut shelves with
mason jars behind, single ceramic mug with steam rising. Slow push-in
toward the mug. Sound: porcelain settles on marble, espresso machine
hisses softly.

[3-7s] Cut to extreme close-up of the mug. Her fingers wrap around the
ceramic, condensation traces the rim, golden window light catches the
foam ripples. She tilts the mug slowly. Sound: gentle slosh, lo-fi piano
swells.

[7-10s] Pull back to medium shot. She lifts the mug, takes a slow sip,
eyes drift to the window, corner of her mouth lifts into a small private
smile. Sound: piano resolves into a soft chime.

Shot on Sony A7S3, natural skin tone, slight film grain, documentary
realism. Face stable, no distortion, motion fluid, no plastic skin,
no oversaturated colors, no on-screen text.
```

## Why the fix works

| Change | Reason |
|---|---|
| Removed generic quality tokens | They do not give Seedance concrete visual instructions |
| Added Sony A7S3 | Named cameras carry stronger implicit image priors |
| Added 3 time segments | Seedance follows short, bounded temporal beats more reliably |
| Added color + material + light details | Each scene now has visible anchors |
| Added one camera move per segment | Motion is directed without over-constraining the model |
| Added audio atmosphere at the top | Sound and BGM are easier for the model to preserve |
| Added 5 negatives | The output gets practical constraints without becoming brittle |

Final score: **10/10**.
