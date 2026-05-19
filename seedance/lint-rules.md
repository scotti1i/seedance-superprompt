# Lint Rules — Seedance 2.0 Prompt Anti-Patterns

A catalog of patterns known to hurt Seedance 2.0 output, distilled from
the official guides and ~100 field tests.

This catalog powers two skill workflows:
- **LINT** (`/seedance lint <prompt>`) — report only, no rewriting
- **FIX** (`/seedance fix <prompt>`) — report + auto-rewrite with diff explanation

Each rule:
- **ID** in `R-<category><number>` format
- **Severity**: 🔴 hard violation (high failure rate) / 🟡 soft warning
  (sub-optimal) / 🔵 informational
- **Detection**: a regex pattern or structural condition the linter can
  spot
- **Why bad**: what goes wrong
- **Fix**: how to rewrite
- **Source**: methodology section or test result

Categories:
- `T` — Time segments
- `S` — Scene / spatial
- `C` — Camera
- `J` — Junk tokens / bad style
- `R` — Reference assets / `@` syntax
- `X` — Text overlay / subtitles
- `A` — Audio
- `N` — Negative constraints
- `M` — Mode-specific
- `B` — Budget / limits

---

## R-T01 🔴 Too many time segments

**Detection**: count of `[<N>-<M>s]` segments ≥ 5.

**Why bad**: methodology field tests show ≥5 segments collapse — the model
merges or drops segments silently. Pacing breaks.

**Fix**: merge to 3 segments (most reliable) or 4 (acceptable for
multi-step demos).

**Source**: `methodology.md` § 2 (Segment count).

---

## R-T02 🟡 Segment time gaps or overlaps

**Detection**: any `[X-Ys]` followed by `[A-Bs]` where `A != Y`, or any
overlap.

**Why bad**: confuses the model on pacing; output may freeze at the gap or
double up.

**Fix**: ensure segments are contiguous: `[0-3s]`, `[3-7s]`, `[7-10s]`.

---

## R-T03 🟡 Segment timeline exceeds declared duration

**Detection**: max end-time of segments > declared `duration_s`.

**Why bad**: model silently truncates; you lose your final segment.

**Fix**: ensure all segments fit within `duration_s`. For 10s video,
segments should not extend beyond `[7-10s]`.

---

## R-T04 🟡 Segment timeline shorter than declared duration

**Detection**: max end-time of segments < `duration_s - 2s`.

**Why bad**: model fills the gap with stuttering or extends last segment
unpredictably.

**Fix**: cover the full duration. If 10s, end at `[7-10s]`.

---

## R-S01 🔴 Scene lacks color + material + light triplet

**Detection**: a segment's scene description contains only one of {color
words / material words / light words} or none.

**Why bad**: triplet locks the spatial layer. Without it, output collapses
to AI-generic.

Examples of MISSING triplet:
- "bright bedroom" — only light
- "in a cafe" — none
- "a beach" — none

**Fix**: write three explicit anchors. Examples:
- "beige walls, cream linen sheets, natural sunlight from the right"
- "marble counter, brass espresso machine, warm tungsten pendant light"

**Source**: `methodology.md` § 4.

---

## R-S02 🟡 Vague subject description

**Detection**: subject described with adjectives only ("a pretty girl", "a
beautiful woman", "a man"), missing identity anchors (age, hair, clothing,
features).

**Why bad**: model fills in defaults that are AI-generic and inconsistent
across segments.

**Fix**: specify: age range + hair + clothing + 1 distinguishing feature.

---

## R-C01 🔴 Multiple camera moves stacked in one segment

**Detection**: a single segment contains 2+ camera-move tokens (e.g.
`push-in` AND `pan` AND `orbit`).

**Why bad**: model gets confused and drops some instructions or jitters
between moves.

**Fix**: one camera move per segment. If you need motion variation, split
into separate segments.

**Source**: `methodology.md` § 5.

---

## R-C02 🟡 Camera move missing entirely

**Detection**: no camera-move keyword in the segment.

**Why bad**: model defaults to locked-off, which can feel sterile. Or it
adds random motion unpredictably.

**Fix**: explicitly write one of: `locked-off`, `slow push-in`, `pull back`,
`pan left/right`, `tracking shot`, `handheld follow`, etc.

---

## R-J01 🔴 Junk style tokens

**Detection**: presence of any of these tokens (case-insensitive):
`highly detailed`, `masterpiece`, `8k`, `4k` (as quality flag), `ultra-realistic`,
`ultra realistic`, `photorealistic` (as standalone tag), `award-winning`,
`award winning`, `breathtaking`, `stunning`, `incredible detail`,
`amazing quality`, `cinematic` (standalone, with no other camera context).

**Why bad**: these have been trained AWAY from improving output. They
actively hurt.

**Fix**: replace with specific terms:
- "highly detailed" → "slight film grain" / "sharp focus on subject"
- "8k" → "Shot on Sony A7S3, natural color"
- "cinematic" → "Shot on ARRI Alexa, 35mm film" or "anamorphic lens flare"
- "amazing" / "breathtaking" / "stunning" → describe what's amazing
  specifically

**Source**: `methodology.md` § 11 (Banned tokens).

---

## R-J02 🟡 Adjective chains

**Detection**: 3+ pure-adjective descriptors in a row, e.g., "beautiful,
stunning, gorgeous woman".

**Why bad**: adjective stacking trains the model toward AI-generic
"pretty" mean output instead of unique character.

**Fix**: replace with one concrete trait + one mood + one action.

---

## R-J03 🟡 Missing named camera

**Detection**: no `Shot on <camera>` token or no equivalent film-stock /
lens token.

**Why bad**: without a camera anchor, the model defaults to generic
"cinematic" which is the worst-quality default.

**Fix**: add one of:
- `Shot on iPhone 15 Pro` (UGC)
- `Shot on Sony A7S3` (documentary)
- `Shot on ARRI Alexa` (cinematic premium)
- `35mm film` / `anamorphic lens flare` (vintage cinematic)
- `RED Komodo` (sharp action)

---

## R-R01 🔴 R2V: `@image_N` not re-bound in every segment

**Detection**: R2V mode prompt where `@image1` appears in segment 1 but
subsequent segments use only pronouns ("she", "he", "it", "the cat", "the
woman").

**Why bad**: identity drift between segments — face changes, scene jumps,
character swaps mid-shot.

**Fix**: re-write `@image_N` in every segment, even for the same subject:
- ❌ `[5-11s] She walks forward and stops.`
- ✅ `[5-11s] @image1 walks forward and stops.`

**Source**: `methodology.md` § 6 (Iron rule #1).

---

## R-R02 🔴 Multiple characters disambiguated by numbers

**Detection**: prompt contains `woman 1`, `woman 2`, `person 3`, `美女3`,
etc., when there are 2+ characters.

**Why bad**: the parser may read "woman 3" as "the 3rd one is" — leading
to wrong character count or wrong identities.

**Fix**: use letters: `woman A`, `woman B`, `美女A`, `美女B`.

**Source**: `methodology.md` § 6 (Iron rule #2).

---

## R-R03 🟡 Pronoun without `@` anchor

**Detection**: pronouns (`she`, `he`, `it`, `her`, `his`, `它`, `他`, `她`)
in a multi-character context without `@image_N` glue.

**Why bad**: ambiguous which character the pronoun refers to.

**Fix**: replace pronoun with `@image_N` or with the character letter.

---

## R-R04 🔴 Asset ID used as character binding

**Detection**: prompt text contains a substring matching `asset-[a-z0-9-]+`
and is being treated as a character name.

**Why bad**: model doesn't map asset IDs to characters. It treats them as
gibberish.

**Fix**: explicitly say "image 1 is X, image 2 is Y" and pass images in
that order. Refer to them as `@image1` / `@image2` in the prompt body.

**Source**: `methodology.md` § 6 (Iron rule #3).

---

## R-R05 🟡 Too many `@image_N` references

**Detection**: count of unique `@image_N` references > 5.

**Why bad**: filling all 9 image slots hurts coherence. SA-recommended
sweet spot is 4-5 total assets.

**Fix**: pick the most essential: 1 character + 1 scene + 1 product + 1
optional second character.

---

## R-M01 🔴 I2V mode re-describes the subject

**Detection**: I2V prompt contains subject descriptors like "the same
young woman", "an east-asian girl", "a white cat", or any phrase that
describes who the subject is.

**Why bad**: the model already sees the image. Re-describing causes
identity drift.

**Fix**: remove all subject description. Write only:
- Motion (subtle, layered verbs)
- Camera (one move)
- Audio (Overall + Sound)
- Style (named camera, film grain)
- Negatives (3-5)

**Source**: `methodology.md` § 7.

---

## R-M02 🔴 Frames mode + ref_video / ref_audio

**Detection**: Frames mode prompt accompanied by `@video_N` or `@audio_N`
references.

**Why bad**: `role_mode=frame` is mutually exclusive with ref_video and
ref_audio. API returns 422.

**Fix**: choose one:
1. Frames mode without pacing reference
2. R2V mode with image1 (start frame), image2 (end frame), and ref_video

**Source**: `methodology.md` § 14 (Mutual exclusion table).

---

## R-X01 🟡 On-screen text > 4 words

**Detection**: pattern `On-screen text: '<more than 4 words>'`.

**Why bad**: text rendering success rate drops below 50% above 4 words
and below 20% for 5-8 words.

**Fix**: shorten to ≤ 4 words, or add the text in post-production with
CapCut / Premiere.

**Source**: `methodology.md` § 8.

---

## R-X02 🔴 Chinese subtitle / long Chinese on-screen text

**Detection**: `On-screen text:` value contains 2+ Chinese characters.

**Why bad**: Chinese text rendering success rate is <20% regardless of
length. Generates gibberish characters.

**Fix**: do not request Chinese on-screen text. Add subtitles in post.

---

## R-X03 🟡 Em-dash in prompt

**Detection**: any `—` character in the prompt body.

**Why bad**: em-dash truncates the following content — the model may
ignore everything after it.

**Fix**: replace `—` with `,` or `.` or split into separate sentences.

---

## R-X04 🟡 Brand / proper noun expected as on-screen text

**Detection**: `On-screen text:` contains a capitalized brand or proper
noun (uppercase tokens like `NIKE`, `Stanley`, `TRENZ`).

**Why bad**: brand-name rendering success rate <20%.

**Fix**: add brand-name overlays in post-production.

---

## R-A01 🔴 Missing `Overall audio atmosphere` declaration

**Detection**: no `Overall audio atmosphere:` (or equivalent) on the
second line.

**Why bad**: BGM declarations buried inside `[X-Ys]` segments are
frequently ignored.

**Fix**: add `Overall audio atmosphere: ...` as the second line, right
after the orientation/ratio line.

**Source**: `methodology.md` § 9.

---

## R-A02 🟡 Voiceover described but not quoted

**Detection**: `Voiceover:` followed by a description of what's said,
without quotation marks.

**Why bad**: model has no script to read. Generates random or no voice.

**Fix**: `Voiceover: 'exact words to say.'` Always quote.

---

## R-A03 🟡 Voiceover too long for segment

**Detection**: voiceover word count exceeds ~2.5 words per second of
segment.

**Why bad**: model compresses to 2× speed → robot voice.

**Fix**: shorten voiceover, or extend the segment.

---

## R-N01 🟡 Negative constraints exceed 5

**Detection**: count of negatives (`no <thing>` or equivalent) > 5.

**Why bad**: over-constraining → output stiff or fails.

**Fix**: pick the 3-5 most relevant negatives. Drop generic ones.

---

## R-N02 🔵 Generic instead of specific negative

**Detection**: negatives like "no bad output", "no errors", "no
imperfections".

**Why bad**: vague negatives don't help.

**Fix**: replace with specific anti-patterns:
- "no plastic skin"
- "no waxy face"
- "no oversaturated colors"
- "no on-screen text"
- "no body distortion"
- "no extra fingers"

---

## R-B01 🔴 Prompt exceeds hard length limit

**Detection**: prompt > 1000 English words OR > 500 Chinese characters OR
> 2000 UTF-8 bytes.

**Why bad**: model truncates silently, usually losing the last segment.

**Fix**: tighten the prompt. Drop redundant adjectives.

**Source**: `methodology.md` § 2 (Hard limits).

---

## R-B02 🟡 Prompt below recommended length for duration

**Detection**: chars / duration < 50 (very sparse).

**Why bad**: model has too little instruction; output becomes random.

**Fix**: per-second char budget ~50-80 chars. Add scene detail / motion
sequencing.

---

## R-B03 🟡 Duration outside stable range

**Detection**: `duration_s < 4` or `duration_s > 15`.

**Why bad**: Seedance API requires 4-15s; outside is rejected. Even
within range, 5 / 10 / 15 are most stable.

**Fix**: pick 5, 10, or 15. Use 4-9 / 11-14 only when needed.

---

## R-B04 🟡 ref_video total duration > 15s

**Detection**: sum of all `@video_N` durations > 15s.

**Why bad**: API hard limit.

**Fix**: trim with `ffmpeg -ss 0 -t 15 -c copy ...`

---

## How linter should output

For each rule that fires:

```
🔴 R-T01: 6 time segments found, ≥5 collapse risk.
   Located: lines 4, 6, 8, 10, 12, 14
   Fix: merge to 3 segments (suggested grouping: [0-5s] [5-10s] [10-15s])

🔴 R-J01: junk token "highly detailed" found.
   Located: line 18
   Fix: replace with named camera or specific lighting term

🟡 R-X01: on-screen text "Buy now and save big" — 5 words, ~50% success.
   Located: line 7
   Fix: shorten to ≤4 words or add in post-production

🟢 R-R01: R2V @image rebinding verified across all 3 segments
```

Final score: count reds (× 3 weight) + yellows (× 1 weight) → grade 1-10.

---

## See also

- `methodology.md` — full methodology with sources
- `examples/bad/` — concrete violations of each rule
- `examples/good/` — examples that pass all rules
