---
name: seedance
description: Seedance 2.0 prompt engineer. Three modes: WRITE (natural language → production-grade prompt), LINT (diagnose anti-patterns in existing prompt), FIX (auto-rewrite a bad prompt into a good one with diff explanation). Auto-picks T2V/I2V/R2V/V2V/Frames, enforces eight-element formula, three-segment timeline, @-syntax, named cameras, and negative constraints. Output pastes cleanly into ANY Seedance client (jimeng.jianying.com, dreamina.capcut.com, Volcengine API, BytePlus, third-party wrappers). Zero CLI dependency.
argument-hint: <natural-language request> | lint <prompt> | fix <prompt> [--mode t2v|i2v|r2v|v2v|frames] [--duration N] [--ratio R] [--style ...] [--no-text] [--plan-only]
---

# /seedance - Seedance 2.0 Prompt Engineer

Turn a sentence into a production-grade Seedance 2.0 prompt. Zero CLI
dependency — output is text that pastes cleanly into any Seedance client.

**Methodology source**: `methodology.md` (compressed from the official
ByteDance / Volcano Engine SOT).
**Mode handbooks**: `modes/{t2v,i2v,r2v,v2v,frames}.md`
**Templates**: `templates/INDEX.md`
**Lint rules**: `lint-rules.md`

---

## What this skill does

Three independent workflows. Pick by user intent.

### WRITE mode (default)

User input: a natural-language video request.
Skill output: a freshly engineered prompt + recommended parameters.

> "10s vertical TikTok-style vlog of a girl drinking coffee in a cozy cafe"
> →
> Parses → picks mode (T2V/I2V/R2V/V2V/Frames) → writes the prompt
> following methodology → returns parameters + client list.

### LINT mode (`/seedance lint <prompt>` or "audit this prompt")

User input: an existing prompt.
Skill output: rule-by-rule report (no rewriting).

> ```
> A beautiful young woman drinking coffee, cinematic, 8k, masterpiece
> ```
> →
> 🔴 R-J01: junk tokens `cinematic` / `8k` / `masterpiece` — replace with
>          named camera
> 🔴 R-S01: missing color + material + light triplet
> 🔴 R-T01: no time segments — pacing will be random
> 🔴 R-A01: no `Overall audio atmosphere`
> 🟢 score: 1/10

### FIX mode (`/seedance fix <prompt>` or "fix this prompt")

User input: a bad prompt.
Skill output: lint report + REWRITTEN prompt + diff explanation
(what changed and why).

This is the highest-value workflow. Users with no Seedance background can
type any rough idea and get a production-grade output back, with the
engineering work explained. See "FIX workflow" below.

---

## What every workflow includes

1. **Mode selection** (or respected if user forces with `--mode`)
2. **Parameters** (duration, ratio, model variant) with cost-aware defaults
3. **Client list** — where the prompt can be pasted
4. **Methodology citations** — which rules from `methodology.md` apply

---

## What this skill does NOT do

- **Does not call any API** — the skill outputs text, the user picks where
  to run it.
- **Does not require a CLI** — no `dreamina`, `volcengine`, or any other
  binary needed.
- **Does not need authentication** — pure prompt engineering.
- **Does not generate or store videos** — the user is responsible for
  generation and storage.

---

## Mode decision tree

Apply in order — first match wins:

| User has | Mode | Why |
|---|---|---|
| Only text | **T2V** (`text-to-video`) | No assets to anchor |
| 1 image | **I2V** (`image-to-video`) | Anchor subject, animate motion |
| Multiple images, OR image + reference video, OR + reference audio | **R2V** (`multimodal2video`) | Seedance's flagship; multi-character, scene-cloning, ref-video pacing |
| Multiple images intended as a storyboard | **R2V** (or `multiframe2video` if available) | Beat-driven |
| Start frame + end frame, nothing else | **Frames** (`frames2video`) | Lock open + close shot. `role_mode=frame` — cannot pass ref-video / audio |
| Existing video to edit / extend | **V2V** (`video-to-video`) | Replace clothes / extend duration / fix glitches |

Detailed handbooks in `modes/`.

---

## Workflow (step-by-step)

### Step 1 — Parse request

Extract into a YAML mental model (don't print it unless `--debug`):

```yaml
scene:          # one-line scenario
subjects:       # 1 woman / 1 cat / 2 people ...
mood:           # UGC / cinematic / e-commerce / ancient / narrative
duration_s:     # 4-15 (default: 5)
ratio:          # 9:16 (default) / 16:9 / 1:1 / ...
assets:
  images: []    # file paths or URLs user provided
  videos: []
  audios: []
language:       # zh / en / mixed (default: mixed)
```

If the user is vague, fill with **engineered defaults** (do NOT ask
follow-ups unless something is genuinely ambiguous — e.g. portrait vs
landscape orientation).

### Step 2 — Pick mode

Apply the decision tree above.

### Step 3 — Write the prompt

Follow `methodology.md` strictly. The output prompt structure:

```
{Vertical|Horizontal} video {ratio}. Overall audio atmosphere: {global BGM + VO + ambience}.

[0-Xs] {Shot type}. {Subject + concrete verbs}. {Color + material + light}. {Single camera move}. Sound: {sfx}.

[X-Ys] Cut to {new shot}. {New beat}. Sound: ...

[Y-Zs] {Closing shot}. {Final beat}. Sound: ...

Shot on {named camera}. {Style tags}. {3-5 specific negatives}.
```

#### Hard rules (auto-check before output)

- [ ] First line: orientation + ratio
- [ ] Second line: `Overall audio atmosphere:` (omit only on `--no-audio`)
- [ ] **3 segments** (not 5+ — they collapse)
- [ ] Every scene has color + material + light
- [ ] Verbs concrete (not "interacts with", "shows", "examines")
- [ ] One camera move per segment (no stacked push + pan + orbit)
- [ ] Named camera at end (Sony A7S3 / iPhone 15 Pro / ARRI Alexa / 35mm
      film) instead of "cinematic 8k"
- [ ] 3-5 specific negatives (more → over-constrain)
- [ ] **No banned tokens**: `highly detailed`, `masterpiece`, `8k`,
      `ultra-realistic`, `amazing`, `breathtaking`, `stunning`
- [ ] R2V: every `@image_N` re-bound in every segment
- [ ] R2V multi-character: letters A/B/C, not numbers
- [ ] I2V: **no subject description** (model already sees the image)
- [ ] Text overlay ≤4 common English words per line
- [ ] `Voiceover: '...'` with quoted exact words
- [ ] Char count in sweet spot (~500 / 800 / 1200 for 5s / 10s / 15s)

### Step 4 — Output format

Always use this structure (skill output is markdown):

```markdown
## Recommended setup

- **Mode**: <T2V / I2V / R2V / V2V / Frames>
- **Duration**: <N>s
- **Aspect ratio**: <r>
- **Model variant**: seedance2.0fast (cheap iteration) | seedance2.0 (hero)
- **Where to run** (pick one):
  - https://jimeng.jianying.com (China)
  - https://dreamina.capcut.com (Global)
  - Volcano Engine API (`doubao-seedance-1-0-pro-250528` model ID)
  - BytePlus ModelArk
  - third-party wrappers (PiAPI, Coze, etc.)

## Prompt (paste this)

​```
<full engineered prompt>
​```

## Engineering notes

- Why I picked <mode>
- Which rules from methodology.md this prompt obeys
- Anti-patterns explicitly avoided

## Risks and known badcases

- Things this prompt may fail at
- What to inspect in the first output

## (Optional) Auto-lint report

Only included if `--lint` flag or if any rule trips:
- <rule ID>: <issue> → <fix>
```

### Step 5 — Optional: lint pass

If `--lint` is set, run `lint-rules.md` rules against the generated prompt
and append a lint report.

---

## Precision-control knobs

Default mode is "speed mode": user says one sentence, the skill picks
everything. But the user can dial in any level of precision:

| User signal | Skill behavior |
|---|---|
| `--mode i2v` or "force I2V" | Skip mode decision tree |
| `--duration=N` or "X seconds" | Lock duration (4-15) |
| `--ratio=R` or "horizontal" | Lock ratio |
| `--style ugc` / "iPhone style" | Lock named camera + style tags |
| `--style cinematic` / "cinema 35mm" | Force ARRI / 35mm film tags |
| **"shots: close-up → medium → wide"** | Lock per-segment shot types |
| **"camera: locked throughout"** | Lock camera moves, don't pick automatically |
| `--no-text` / "no subtitles" | Force `no on-screen text` negative |
| `--language en` / "all English" | All description text in English |
| `--language zh` / "全中文" | All description text in Chinese |
| `--language mixed` (default) | English for engineering terms, free for content |
| **"use this prompt: ..."** | User supplies own prompt; skill only picks mode + parameters |
| `lint <prompt>` or "audit my prompt" | Run lint pass, report only (no rewrite) |
| **`fix <prompt>` or "fix my prompt"** | **Run lint + rewrite + diff explanation** |
| `--plan-only` (default) | Output prompt + advice; user runs it themselves |

The skill **never** auto-executes anything. It is prompt-only.

---

## Template shortcut

If the user request matches a known template (e.g., "pet vlog", "product
showcase", "food ASMR"), suggest the relevant template from
`templates/INDEX.md` and fill its variables. Faster than writing from
scratch.

---

## Lint workflow (`lint <prompt>`)

When invoked as `/seedance lint <prompt>` or "audit this prompt":

1. Parse the prompt into structural pieces (segments, subjects, camera
   moves, negatives, etc.)
2. Apply each rule in `lint-rules.md`
3. Output a report only — **do not rewrite**

```markdown
## Lint report

- 🔴 **R-T01**: 6 time segments detected (≥5 collapse risk).
  → Merge to 3 segments.
- 🔴 **R-J03**: "highly detailed" found — junk token.
  → Replace with named camera or specific lighting term.
- 🟡 **R-X04**: text overlay "Buy now and save 50% off today" — 8 words.
  → Shorten to ≤4, or add post-production.
- 🟢 R2V `@image` rebinding correct in all 3 segments.

Score: 6/10 — fix 2 reds before submitting.
```

---

## FIX workflow (`fix <prompt>`) — the killer feature

**Highest-value workflow.** Users with a rough or broken prompt get a
production-grade rewrite back, with the engineering work explained.

When invoked as `/seedance fix <prompt>` or "fix my prompt" or "rewrite
this":

### Step F1 — Lint the input

Run every rule in `lint-rules.md` against the user's prompt. Collect:
- 🔴 hard violations (R-T01 timeline, R-J01 junk, R-S01 triplet, R-A01
  audio, R-M01 I2V redescription, R-R01 R2V rebinding, etc.)
- 🟡 soft warnings
- Score (1-10)

### Step F2 — Infer intent

From the user's broken prompt, extract their actual intent:
- What's the scene about? (look past the junk tokens)
- What's the implied duration / ratio? (vertical phrases → 9:16; "5 sec"
  → 5s; if absent, default to 10s 9:16)
- What mood do they want? (UGC / cinematic / e-commerce / etc.)
- Are there any assets implied? (mentions of "image" / "video" / "ref" →
  switch mode; otherwise default to T2V)
- What language did they write in? (preserve language for content
  description, switch engineering tags to English regardless)

### Step F3 — Rewrite

Write the new prompt **from scratch** using methodology.md, NOT by
patching the user's text. Patching produces Frankenstein output.

The rewrite must:
- Add `Vertical|Horizontal video <ratio>` first line
- Add `Overall audio atmosphere: ...` second line
- Build 3 segments with proper `[0-Xs] [X-Ys] [Y-Zs]`
- Each scene gets color + material + light triplet
- Use concrete verbs (replace "doing X", "interacts with X" with action
  sequences)
- One camera move per segment
- End with `Shot on <named camera>. <style>. <3-5 negatives>.`
- Drop every banned token from the input
- Strip every adjective chain
- For I2V: never re-describe the subject
- For R2V: bind `@image_N` in every segment

### Step F4 — Output

Use this fixed structure:

```markdown
## Lint report — score: X/10

- 🔴 R-XXX: <issue>. → <fix>
- ...

## Fixed prompt (paste this)

​```
<rewritten engineered prompt>
​```

## What changed and why

| Removed | Added | Reason |
|---|---|---|
| `cinematic` / `8k` / `masterpiece` | `Shot on Sony A7S3, slight film grain` | junk tokens (R-J01); named camera invokes documentary realism |
| `beautiful young woman` | `young east-asian woman with shoulder-length dark hair, cream sweater` | vague subject (R-S02); concrete identity anchor |
| `in a cozy cafe` | `marble counter, walnut shelves, warm tungsten pendant` | missing color + material + light (R-S01) |
| (no time segments) | `[0-3s] [3-7s] [7-10s]` | timeline missing (R-T01) |
| (no audio declaration) | `Overall audio atmosphere: warm lo-fi piano...` | BGM ignored without declaration (R-A01) |
| (no camera move) | per-segment: push-in → locked → pull back | one move per segment (R-C01) |
| (no negatives) | `no plastic skin, no oversaturated colors, no on-screen text` | 3-5 specific negatives (R-N01) |

## Mode chosen and parameters

- Mode: T2V (no anchor assets in original prompt)
- Duration: 10s, Aspect ratio: 9:16, Model: seedance2.0fast
- Paste into: jimeng.jianying.com / dreamina.capcut.com / Volcengine API /
  BytePlus / third-party

## Risks / open questions

- The original prompt did not specify subject identity beyond "beautiful
  young woman". I picked one specific anchor; if you want a different
  ethnicity / age / clothing, change the description in segment 1.
- The original prompt did not specify duration. I picked 10s as the most
  stable archetype; tell me if you want 5s / 15s.
```

### FIX rules — DO and DON'T

**DO**:
- Read past the noise to infer real intent
- Rewrite from scratch using methodology
- Explain each change with a rule citation
- Ask about ambiguities only if structurally unclear (e.g., "no duration
  specified — defaulting to 10s; tell me if you want different")

**DON'T**:
- Patch the user's text (Frankenstein output)
- Strip information they explicitly wanted (if they say "red lipstick"
  keep that)
- Add things they didn't imply (no inventing a third character)
- Lecture them — keep diff explanation crisp, one row per change
- Run unless explicitly asked (mode is `fix`, not default)

---

## Output language policy

- **Skill responses** (this skill's own commentary): user's language.
- **Engineered prompts**: default `mixed` (engineering terms in English,
  content in user's language). User can override with `--language`.
- **Reasoning**: prefer Chinese if the user wrote in Chinese, English if
  they wrote in English.

---

## When to push back

If the request is fundamentally incompatible with Seedance 2.0, say so:

- Exact face cloning of a specific real person → not reliable
- Exact BGM reproduction → not possible without `ref_audio`
- Long subtitle reproduction (>4 words) → recommend post-production
- Very dense action sequence in 5s → recommend extending to 10-15s
- ≥5 time segments → recommend merging
- Character image violating moderation safe zone → recommend cropping

These honest pushbacks save the user from burning credits.

---

## See also

- `methodology.md` — the spine
- `modes/{t2v,i2v,r2v,v2v,frames}.md` — per-mode handbooks
- `decision-trees/which-mode.md` — visualized mode picker
- `decision-trees/duration-budget.md` — char count vs duration
- `decision-trees/safety-zone.md` — content moderation guide
- `lint-rules.md` — anti-pattern catalog
- `templates/INDEX.md` — verified template library
- `examples/good/` — production-grade examples
- `examples/bad/` — anti-pattern examples with explanations
