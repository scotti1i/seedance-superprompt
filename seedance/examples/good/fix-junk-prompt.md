# Killer Demo — FIX a junk prompt

The single most viral demo: input a typical newbie prompt, output a
production-grade one with full diff explanation.

This is the hero case study for the skill's `fix` workflow.

---

## Input (typical newbie prompt)

What a creator might type after 5 seconds of thought:

```
A beautiful young woman drinking coffee in a cozy cafe, cinematic,
highly detailed, masterpiece, 8k, very beautiful, amazing quality,
sunlight, professional photography
```

Length: ~150 chars. Reads polished. Generates garbage.

---

## Skill output — `/seedance fix <prompt>`

### Lint report — score: 1/10

| Rule | Severity | Issue |
|---|---|---|
| R-J01 | 🔴 | Junk tokens: `cinematic`, `highly detailed`, `masterpiece`, `8k`, `amazing quality` |
| R-J02 | 🟡 | Adjective chain: "beautiful... very beautiful... amazing... professional" |
| R-J03 | 🔴 | No named camera → model defaults to AI generic |
| R-S01 | 🔴 | Missing color + material + light triplet ("cozy cafe" is not a triplet) |
| R-S02 | 🟡 | Vague subject ("beautiful young woman") |
| R-T01 | 🔴 | No time segments — pacing is random |
| R-A01 | 🔴 | No `Overall audio atmosphere` — BGM will be ignored |
| R-C02 | 🟡 | No camera move declared |
| R-N01 | 🔴 | No negatives — no quality guardrails |

### Fixed prompt (paste this)

```
Vertical video 9:16. Overall audio atmosphere: warm lo-fi piano with soft espresso machine hiss, distant cafe chatter, no voiceover.

[0-3s] Medium close-up handheld shot. A young east-asian woman with shoulder-length dark hair, cream cashmere sweater, sits at a marble counter. Warm tungsten pendant light overhead, dark walnut shelves with mason jars behind, single ceramic mug with steam rising. Slow push-in toward the mug. Sound: porcelain settles on marble, espresso machine hisses softly.

[3-7s] Cut to extreme close-up of the mug. Her fingers wrap around the ceramic, condensation traces the rim, golden window light catches the foam ripples. She tilts the mug slowly. Sound: gentle slosh, lo-fi piano swells.

[7-10s] Pull back to medium shot. She lifts the mug, takes a slow sip, eyes drift to the window, corner of her mouth lifts into a small private smile. Sound: piano resolves into a soft chime.

Shot on Sony A7S3, natural skin tone, slight film grain, documentary realism. Face stable, no distortion, motion fluid, no plastic skin, no oversaturated colors, no on-screen text.
```

### What changed and why

| Removed | Added | Rule | Reason |
|---|---|---|---|
| `cinematic` / `highly detailed` / `masterpiece` / `8k` / `amazing quality` | `Shot on Sony A7S3, natural skin tone, slight film grain, documentary realism` | R-J01 / R-J03 | Junk tokens trigger AI-generic defaults; named camera invokes documentary realism aesthetic the model actually knows |
| `beautiful young woman` (vague) | `young east-asian woman with shoulder-length dark hair, cream cashmere sweater` | R-S02 | Concrete subject anchors prevent AI-generic face filling |
| `in a cozy cafe` (one bucket) | `marble counter, dark walnut shelves with mason jars, warm tungsten pendant light overhead` | R-S01 | Color + material + light triplet locks the spatial layer |
| (no segments) | `[0-3s] [3-7s] [7-10s]` three-segment timeline | R-T01 | Without segments, model invents its own pacing — often poorly |
| (no audio declaration) | `Overall audio atmosphere: warm lo-fi piano with soft espresso machine hiss...` (line 2) | R-A01 | BGM declarations buried in mid-prompt get ignored — must be top |
| (no camera) | per-segment: `Slow push-in` → `Cut to extreme close-up` → `Pull back to medium shot` | R-C01 / R-C02 | One camera move per segment; variety across segments |
| (no negatives) | `no plastic skin, no oversaturated colors, no on-screen text` (3 specific) | R-N01 | 3-5 specific negatives = guardrail; >5 over-constrains |
| `sunlight` (vague light) | `warm tungsten pendant light overhead` + `golden window light` (segment 2) | R-S01 | Specific direction + temperature + source |

### Mode chosen and parameters

- **Mode**: T2V (no anchor assets implied in original)
- **Duration**: 10s — sweet spot for 3-segment narrative
- **Aspect ratio**: 9:16 — vertical assumed since the original
  feels mobile-native (no horizontal/landscape signal)
- **Model variant**: seedance2.0fast (iteration), upgrade to
  seedance2.0 Pro for hero shot
- **Paste into**:
  - jimeng.jianying.com (China web)
  - dreamina.capcut.com (Global web)
  - Volcano Engine API (`doubao-seedance-1-0-pro-250528`)
  - BytePlus ModelArk
  - PiAPI / Coze / third-party wrappers

### Risks / open questions

- Original prompt did not specify ethnicity / age range / clothing — I
  picked specific anchors. If you want different, change segment 1's
  subject description.
- Original prompt did not specify duration — defaulted to 10s. Tell me
  if you want 5s (single beat) or 15s (extended narrative).
- Original prompt did not specify aspect ratio — defaulted to 9:16
  (vertical / mobile). Switch to 16:9 if YouTube / horizontal use case.

---

## Why this is the headline demo

| Before | After |
|---|---|
| Score 1/10 | Score 10/10 |
| 9 lint violations | 0 violations |
| ~5 seconds to write | ~30 seconds to read the explanation |
| Generates AI-generic garbage | Generates production-grade vlog footage |
| Wastes 75 credits per attempt | One-shot generation |

This is the demo that fits in a 60-second TikTok / Reels / Twitter video:
- 0:00 type the junk prompt
- 0:10 `/seedance fix`
- 0:20 paste output into jimeng
- 0:50 result video plays

Pure visual transformation. No prior knowledge required.

---

## How to teach this in a tweet

> Most Seedance / Dreamina / 即梦 prompts on the internet look like:
> "a beautiful girl drinking coffee, cinematic, highly detailed, 8k,
>  masterpiece"
>
> They generate slop because they trigger every anti-pattern in the
> official ByteDance prompt guide.
>
> `/seedance fix` takes any of these and rewrites them into a 3-segment,
> named-camera, audio-declared, negative-constrained prompt that scores
> 10/10 against the methodology — and explains every change.
>
> Open-source skill, MIT license, install in 30 seconds.
> github.com/<user>/seedance-prompt

---

## See also

- `../../lint-rules.md` — all 25 rules referenced above
- `../../methodology.md` — the spine behind every fix
- `../bad/junk-tokens.md` — the same junk prompt with manual annotation
