# Seedance 2.0 Superprompt

> A portable prompt skill / knowledge base for writing, auditing, and
> auto-fixing **ByteDance Seedance 2.0** video-generation prompts.
> Primary client: **Claude Code**. Also works as reference for **Codex,
> Cursor, Claude.ai, ChatGPT, Gemini**, or any LLM agent.

**🌐**  [**English**](README.md)  ·  [中文](README.zh-CN.md)

> **Turn any rough video idea — even a junk-token soup — into a
> production-grade Seedance 2.0 prompt that pastes anywhere.**
> Zero CLI · Zero API · Zero credentials.

⭐ [Star on GitHub](https://github.com/scotti1i/seedance-superprompt) · 📚 [Methodology](seedance/methodology.md) · ▶️ [Author's YouTube](https://www.youtube.com/@ScottGlobalAI) · 🇨🇳 [中文](README.zh-CN.md)

> **Status**: solo-maintained by [@scotti1i](https://github.com/scotti1i).
> Free to use, fork, and redistribute (MIT). **PRs not accepted at this
> stage** — please use [Issues](https://github.com/scotti1i/seedance-superprompt/issues)
> for bugs, lint-rule proposals, and template ideas. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## ⚡ Install in 30 seconds

```bash
git clone https://github.com/scotti1i/seedance-superprompt.git
cd seedance-superprompt && bash install.sh
```

Restart Claude Code. Type `/seedance` and you're live.

---

## 🛠️ Works with

This is **portable prompt knowledge**, not Claude Code-locked. The
`SKILL.md` + `methodology.md` + `templates/` + `lint-rules.md` files
work as a reference library inside any AI coding agent.

| Tool | How to use |
|---|---|
| **Claude Code** (primary) | Auto-install via `bash install.sh` → use `/seedance ...` natively |
| **Codex CLI** | Paste `seedance/SKILL.md` content as the agent's system prompt; reference `seedance/methodology.md` for rules |
| **Cursor** | Drop the `seedance/` folder into your project; ask Cursor to read it and follow the workflow in `SKILL.md` |
| **Claude.ai / Claude.app** | Upload `methodology.md` + relevant template as project knowledge |
| **ChatGPT / Gemini / DeepSeek** | Copy `SKILL.md` + `methodology.md` into the chat; ask the model to follow them |
| **Manual** | Read `templates/INDEX.md`, copy a template, fill variables yourself, paste into your Seedance client |

The actual output is **always plain text prompts**. The skill mechanic
is the workflow; the methodology and templates are portable knowledge.

---

## 🎯 What you get (3 modes, 1 skill)

| You type | Skill returns |
|---|---|
| `/seedance 10s vertical cat vlog in afternoon light` | A full 3-segment engineered prompt + parameters + paste-target list |
| `/seedance lint <your prompt>` | A rule-by-rule audit + 1-10 score in 5 seconds |
| `/seedance fix <your bad prompt>` | **Lint report + auto-rewritten prompt + diff explanation** (the killer feature) |

**Bottom line**: stop writing AI-generic prompts. This skill enforces
named cameras (Sony A7S3, ARRI Alexa, iPhone 15 Pro), 3-segment
timelines, color+material+light scene triplets, `@image` rebinding, and
3-5 specific negatives — automatically, every time, citing the official
ByteDance methodology rule for each decision.

The output is plain text. Paste it into:
[即梦](https://jimeng.jianying.com) ·
[CapCut Dreamina](https://dreamina.capcut.com) ·
Volcano Engine API · BytePlus ModelArk ·
PiAPI · Coze · or any third-party wrapper.

---

## ⭐ Headline demo: FIX a junk prompt in 5 seconds

### You type (typical newbie prompt)

```
A beautiful young woman drinking coffee in a cozy cafe, cinematic,
highly detailed, masterpiece, 8k, amazing quality
```

### Skill responds

```
## Lint report — score: 1/10

🔴 R-J01: junk tokens "cinematic" / "highly detailed" / "masterpiece" / "8k"
🔴 R-S01: missing color + material + light triplet
🔴 R-T01: no time segments → pacing will be random
🔴 R-A01: no Overall audio atmosphere → BGM ignored
🔴 R-C02: no camera move
🔴 R-J03: no named camera
🔴 R-N01: no negatives
... (9 violations total)

## Fixed prompt (paste this)

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

## What changed and why

Removed `cinematic` / `8k` / `masterpiece`  →  added `Shot on Sony A7S3`
(R-J01 / R-J03 — junk tokens hurt, named camera invokes documentary realism)

Added 3-segment timeline `[0-3s] [3-7s] [7-10s]`  (R-T01)
Added color + material + light triplet to every scene  (R-S01)
Added one camera move per segment: push-in → locked → pull back  (R-C01)
Added `Overall audio atmosphere:` as line 2  (R-A01)
Added 5 specific negatives at the end  (R-N01)
... (full diff in skill output)

## Mode & parameters
- Mode: T2V (no assets implied)
- Duration: 10s · Ratio: 9:16 · Model: seedance2.0fast
- Paste into: jimeng.jianying.com, dreamina.capcut.com, Volcengine API,
  BytePlus, PiAPI, Coze, etc.
```

**Score: 1/10 → 10/10. ~30 seconds.**

Now paste the fixed prompt into [jimeng.jianying.com](https://jimeng.jianying.com),
[dreamina.capcut.com](https://dreamina.capcut.com), Volcengine API,
BytePlus, or any wrapper. Done.

See the [full case study](seedance/examples/good/fix-junk-prompt.md).

---

## Three workflows

### 1. WRITE — idea → prompt (default)

```
/seedance 10s vertical TikTok-style coffee shop POV in autumn
```
→ Skill picks T2V mode, writes 3-segment engineered prompt, lists clients.

### 2. LINT — audit an existing prompt

```
/seedance lint <your prompt>
```
→ Skill returns rule-by-rule report + 1-10 score. No rewriting.

### 3. FIX — rewrite a bad prompt (the killer feature)

```
/seedance fix <your bad prompt>
```
→ Lint report + REWRITTEN prompt + diff explanation (what changed, why).

All three workflows output text only — paste into your Seedance client of
choice.

---

## Why this exists

Seedance 2.0 by ByteDance is the strongest open multimodal video model
available — but writing good prompts for it is **a different skill** from
writing prompts for SD / Midjourney / Veo. The official guide is ~1100
lines of methodology. The reward for reading it is real (10× better
output) but the entry barrier is high.

This skill compresses that 1100-line SOT into a Claude-driven workflow:

| Problem | Fix |
|---|---|
| Junk tokens like `highly detailed`, `8k`, `masterpiece` hurt output | Lint catches them, replaces with named cameras (Sony A7S3, ARRI Alexa, iPhone 15 Pro, 35mm film) |
| Too many time segments (≥5) silently collapse | Skill forces 3-segment structure |
| I2V users re-describe the subject and cause identity drift | Skill enforces "motion only, never subject" |
| R2V users bind `@image1` once and get face swaps mid-shot | Skill rebinds in every segment |
| Multi-character prompts use `woman 3` and parser breaks | Skill uses letters A/B/C |
| Vague action verbs ("interacts with the product") produce vague output | Skill rewrites to concrete sequences ("rotates the bottle, places it on the table") |

---

## Install

### Option A — One-line install

```bash
curl -fsSL https://raw.githubusercontent.com/scotti1i/seedance-superprompt/main/install.sh | bash
```

### Option B — Manual

```bash
git clone https://github.com/scotti1i/seedance-superprompt.git
cp -r seedance-prompt/seedance ~/.claude/skills/seedance
```

### Option C — Symlink (for contributors)

```bash
git clone https://github.com/scotti1i/seedance-superprompt.git
ln -s "$(pwd)/seedance-prompt/seedance" ~/.claude/skills/seedance
```

After install, restart Claude Code. The skill becomes available as
`/seedance` and is auto-triggered when you mention Seedance / video
generation / Dreamina / 即梦.

---

## Quick start

### 🪄 Fix a bad prompt (most popular)

```
/seedance fix a beautiful girl drinking coffee, cinematic, 8k, masterpiece
```

→ Returns lint report + full rewrite + diff explanation in one pass.
See [the headline demo](seedance/examples/good/fix-junk-prompt.md).

### Speed mode (one sentence)

```
/seedance 10s vertical cat playing with a ball in soft afternoon light
```

→ Skill auto-picks T2V, 9:16, 10s, seedance2.0fast, generates a 3-segment
engineered prompt, and lists where you can paste it.

### Precision mode (lock specific knobs)

```
/seedance product showcase 8s 16:9 cinematic, ARRI Alexa look, no subtitles
```

→ Skill respects every constraint you specify.

### Templates (pre-built scenarios)

```
/seedance use template pet-vlog, my pet is a tabby cat, product is a blue
collar from Petsmart
```

→ Substitutes variables in the verified `pet-vlog` template.

### Lint only (don't rewrite)

```
/seedance lint <your prompt>
```

→ Runs 25 lint rules and reports a grade. No rewriting.

---

## Features

### 🪄 Three workflows in one skill
- **WRITE**: idea → engineered prompt
- **LINT**: audit any prompt against 25 anti-pattern rules
- **FIX**: rewrite a bad prompt into a good one, with diff explanation

### 🎯 Auto mode selection
Picks T2V / I2V / R2V / V2V / Frames based on what assets you have.

### 📐 Methodology enforcement
25 lint rules distilled from the official ByteDance / Volcano Engine
prompting guides and community-published field reports.

### 📚 Template library
8 verified production templates: pet vlog, product showcase, morning
routine, city walk, food ASMR, ancient drama, unboxing, before/after.

### 🌍 Multi-client output
One prompt format works across:
- [jimeng.jianying.com](https://jimeng.jianying.com) (China web)
- [dreamina.capcut.com](https://dreamina.capcut.com) (Global web)
- Volcano Engine API (`doubao-seedance-1-0-pro-250528`)
- BytePlus ModelArk
- PiAPI, Coze, 即梦助手 (third-party wrappers)
- ByteDance internal CLI (if you have access)

### 🚫 Zero infrastructure
No API calls. No credentials. No payment. Pure prompt engineering.

### 🌐 Bilingual
Prompts in Chinese, English, or mixed. Engineering tags stay English
(industry standard), content language is your choice.

---

## Repository structure

```
seedance-prompt/
├── README.md            ← you are here
├── LICENSE              ← MIT
├── CONTRIBUTING.md      ← how to add templates / lint rules
├── install.sh           ← one-line installer
├── TESTING.md           ← end-to-end test scenarios
├── .github/             ← issue and PR templates
└── seedance/            ← the skill itself (symlink target)
    ├── SKILL.md         ← Claude entry point
    ├── methodology.md   ← 500-line distilled SOT
    ├── modes/           ← per-mode handbooks (t2v / i2v / r2v / v2v / frames)
    ├── decision-trees/  ← visual pickers (mode / duration / camera / safety)
    ├── templates/       ← verified production templates
    ├── examples/        ← good / bad prompt examples
    └── lint-rules.md    ← 25 anti-pattern catalog
```

---

## Template catalog

| Template | Mode | Duration | Use case |
|---|---|---|---|
| [pet-vlog](seedance/templates/pet-vlog.md) | R2V | 15s | Pet outdoor walk + product showcase (TikTok viral) |
| [product-showcase](seedance/templates/product-showcase.md) | R2V | 10s | Hero rotation + features + brand close-up |
| [morning-routine](seedance/templates/morning-routine.md) | T2V | 10s | Slow-cinematic lifestyle vignette |
| [city-walk](seedance/templates/city-walk.md) | T2V | 10s | POV travel through aesthetic district |
| [food-asmr](seedance/templates/food-asmr.md) | T2V | 8s | Food close-up with ASMR sound |
| [ancient-drama](seedance/templates/ancient-drama.md) | R2V | 15s | Wuxia two-character confrontation |
| [unboxing](seedance/templates/unboxing.md) | R2V | 10s | POV unboxing reveal |
| [before-after](seedance/templates/before-after.md) | R2V | 8s | Cosmetics / transformation reveal |

Add yours with a PR — see [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Methodology summary

The skill enforces:

1. **Eight-element formula** — subject + action + scene + light + camera +
   style + quality + constraints, in that order
2. **Three-segment timeline** — `[0-Xs]` `[X-Ys]` `[Y-Zs]`. ≥5 collapses.
3. **Scene triplet** — color + material + light for every scene
4. **One camera move per segment** — no stacking
5. **Named camera, not "cinematic"** — Sony A7S3, ARRI Alexa, iPhone 15
   Pro, 35mm film
6. **R2V: `@image_N` rebound every segment** — pronouns lose anchor
7. **Multi-character: letters A/B/C** — numbers confuse the parser
8. **I2V: never re-describe subject** — image already has the info
9. **Audio atmosphere declared at top** — buried BGM gets ignored
10. **3-5 specific negatives** — more over-constrains

Full details: [methodology.md](seedance/methodology.md).

---

## What this skill is NOT

- ❌ A wrapper around the dreamina / volcengine CLI
- ❌ A video generator (it produces prompts, not videos)
- ❌ A paid service (no credits, no API keys)
- ❌ A guarantee — Seedance is non-deterministic; expect 2-5 retries for
  hero shots
- ❌ Affiliated with ByteDance / Volcano Engine / Dreamina / Jianying

---

## 👤 About the author

**ScottLi** ([@scotti1i](https://github.com/scotti1i)) — AI builder
working on TikTok Shop, cross-border e-commerce, and AI video tooling.
I open-source the methodology tools I build for my own work.

If this skill helps you, give it a ⭐ on GitHub and reach out via any
channel below — I read everything.

| Platform | Handle |
|---|---|
| 📧 Email | [yifanlitju@gmail.com](mailto:yifanlitju@gmail.com) |
| ▶️ YouTube | [@ScottGlobalAI](https://www.youtube.com/@ScottGlobalAI) |
| 🎵 抖音 (Douyin) | [SCT出海Scott](https://www.douyin.com/user/MS4wLjABAAAAwB0Jkhs1X4lC6k2OhohpTuQgtxoZQiYHmGhqleB9kGQ) |
| 🐙 GitHub | [@scotti1i](https://github.com/scotti1i) |

### Sponsor / hire

If your team is building with Seedance 2.0 or other ByteDance video
models at scale and wants a custom prompt-engineering audit, methodology
training, or workflow integration — get in touch via any of the channels
above.

---

## Contributing

The skill gets stronger every time someone:

- adds a new verified template
- finds a new anti-pattern and writes a lint rule
- updates methodology after a Seedance model version bump
- contributes a good / bad example

See [CONTRIBUTING.md](CONTRIBUTING.md) and the
[issue templates](.github/ISSUE_TEMPLATE/) to get started.

---

## Roadmap

- [ ] Add 12 more templates (target: 20 total)
- [ ] Cross-model adapters (when Veo / Sora prompts diverge)
- [ ] Auto-update when official ByteDance SOT changes
- [ ] Video-output benchmark suite (with anonymized eval samples)
- [ ] CLI companion (lint as a standalone binary)

---

## License

MIT. See [LICENSE](LICENSE).

---

## Acknowledgments

- ByteDance Seedance team for the official prompting guide
- Volcano Engine docs team for the API reference
- The community of creators who've shared field-tested prompts on
  Lark, WeChat, and small Discord servers — your write-ups made this
  possible
