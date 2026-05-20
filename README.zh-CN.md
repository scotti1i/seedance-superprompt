# Seedance 2.0 Superprompt

> 一个可移植的提示词工具 / 知识库，专门用来**写、审计、一键修复**字节
> **Seedance 2.0** 视频生成模型的提示词。
> 主入口：**Claude Code**。也可作为参考库给 **Codex / Cursor /
> Claude.ai / ChatGPT / Gemini** 等任何 LLM 工具使用。

**🌐**  [English](README.md)  ·  [**中文**](README.zh-CN.md)

> **把任意视频想法——哪怕是充满 junk token 的烂 prompt——一键变成
> production-grade 的 Seedance 2.0 提示词，粘到任何客户端都能跑。**
> 零 CLI · 零 API · 零凭证。

⭐ [GitHub 加星](https://github.com/scotti1i/seedance-superprompt) · 📚 [方法论](seedance/methodology.md) · ▶️ [作者 YouTube](https://www.youtube.com/@ScottGlobalAI) · 🇬🇧 [English](README.md)

> **状态**：由 [@scotti1i](https://github.com/scotti1i) 个人维护。
> 任何人可以免费使用、fork、再发布（MIT）。**当前不接受 PR**——
> bug / lint 规则建议 / 新模板提案请走
> [Issues](https://github.com/scotti1i/seedance-superprompt/issues)。
> 详情见 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## ⚡ 30 秒安装

```bash
git clone https://github.com/scotti1i/seedance-superprompt.git
cd seedance-superprompt && bash install.sh
```

重启 Claude Code。输入 `/seedance` 就能用。

---

## 🛠️ 适用的 AI 工具

这是**可移植的提示词知识**，不绑定 Claude Code。
`SKILL.md` + `methodology.md` + `templates/` + `lint-rules.md` 这套
文件可以作为参考库放进任意 AI 编程助手。

| 工具 | 怎么用 |
|---|---|
| **Claude Code**（主入口）| `bash install.sh` 自动安装 → 原生使用 `/seedance ...` |
| **Codex CLI** | 把 `seedance/SKILL.md` 内容当 system prompt 喂给 agent，引用 `seedance/methodology.md` 作为规则库 |
| **Cursor** | 把 `seedance/` 整个文件夹放进项目，让 Cursor 读取并按 `SKILL.md` 工作流执行 |
| **Claude.ai / Claude.app** | 把 `methodology.md` + 相关模板作为 project knowledge 上传 |
| **ChatGPT / Gemini / DeepSeek** | 把 `SKILL.md` + `methodology.md` 内容贴进对话，让模型遵循 |
| **手动** | 直接看 `templates/INDEX.md`，复制模板填变量，粘到你用的 Seedance 客户端 |

输出**永远是纯文本 prompt**。Skill 的机制是工作流；方法论和模板是可移植
的知识资产。

---

## 🎯 你能拿到什么（1 个 skill，3 种模式）

| 你输入 | Skill 返回 |
|---|---|
| `/seedance 10s 竖屏 一只猫在午后阳光下玩球` | 一份 3 段式工程化 prompt + 参数 + 可粘贴客户端清单 |
| `/seedance lint <你的 prompt>` | 25 条规则逐条审计 + 1-10 评分（5 秒出报告）|
| `/seedance fix <你的烂 prompt>` | **Lint 报告 + 自动重写的 prompt + 完整 diff 解释**（杀手锏）|

**一句话**：别再写 AI 味浓的烂 prompt 了。Skill 自动强制使用命名相机
（Sony A7S3 / ARRI Alexa / iPhone 15 Pro）、3 段时间轴、颜色+材质+光源
三件套、`@图片N` 每段绑定、3-5 个具体负面词——每次都按字节官方方法论
执行，并给出每个决策的规则引用。

输出是纯文本，可粘到：
[即梦](https://jimeng.jianying.com) ·
[CapCut Dreamina](https://dreamina.capcut.com) ·
火山引擎 API · BytePlus ModelArk ·
PiAPI · Coze · 或任何第三方 wrapper。

---

## ⭐ 头条演示：5 秒修好一个烂 prompt

### 你输入（典型新手 prompt）

```
A beautiful young woman drinking coffee in a cozy cafe, cinematic,
highly detailed, masterpiece, 8k, amazing quality
```

### Skill 输出

```
## Lint report — 评分: 1/10

🔴 R-J01: junk token "cinematic" / "highly detailed" / "masterpiece" / "8k"
🔴 R-S01: 缺颜色+材质+光源三件套
🔴 R-T01: 没时间轴分段 → 模型自己瞎拍
🔴 R-A01: 没 Overall audio atmosphere → BGM 会被忽略
🔴 R-C02: 没明确运镜
🔴 R-J03: 没命名相机
🔴 R-N01: 没负面词
... (9 处违规)

## 修复后的 prompt（粘这个）

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
eyes drift to the window, corner of her mouth lifts into 一个 small
private smile. Sound: piano resolves into a soft chime.

Shot on Sony A7S3, natural skin tone, slight film grain, documentary
realism. Face stable, no distortion, motion fluid, no plastic skin,
no oversaturated colors, no on-screen text.

## 改了什么、为什么改

删 `cinematic` / `8k` / `masterpiece` → 加 `Shot on Sony A7S3`
(R-J01 / R-J03 — junk token 反向起作用；命名相机调动模型隐式知识)

加 3 段时间轴 `[0-3s] [3-7s] [7-10s]` (R-T01)
每段加颜色+材质+光源三件套 (R-S01)
每段加一个运镜：push-in → 锁定 → pull back (R-C01)
首行声明 `Overall audio atmosphere:` (R-A01)
末尾加 5 个具体负面词 (R-N01)
... (完整 diff 在 skill 输出里)

## 模式 & 参数
- 模式: T2V（没有锚定素材）
- 时长: 10s · 比例: 9:16 · 模型: seedance2.0fast
- 可粘到: jimeng.jianying.com / dreamina.capcut.com / 火山引擎 API /
  BytePlus / PiAPI / Coze 等
```

**评分: 1/10 → 10/10，30 秒搞定。**

然后把修复后的 prompt 粘到 [即梦](https://jimeng.jianying.com)、
[CapCut Dreamina](https://dreamina.capcut.com)、火山引擎 API、
BytePlus 或任意第三方 wrapper，开跑。

完整案例研究：[fix-junk-prompt.md](seedance/examples/good/fix-junk-prompt.md)

---

## 三种工作流

### 1. WRITE — 想法 → prompt（默认）

```
/seedance 10s 竖屏 抖音风格 咖啡馆 POV 秋天
```
→ 自动选 T2V 模式，写出 3 段工程化 prompt，列出可粘贴的客户端。

### 2. LINT — 审计已有 prompt

```
/seedance lint <你的 prompt>
```
→ 25 条规则逐条检查，给出 1-10 评分。**只检测，不重写**。

### 3. FIX — 一键修复烂 prompt（杀手锏）

```
/seedance fix <你的烂 prompt>
```
→ Lint 报告 + 重写的 prompt + 完整 diff 解释（改了什么、为什么）。

三种工作流都**只输出文字**——你想粘到哪个 Seedance 客户端都行。

---

## 为什么有这个

ByteDance 的 Seedance 2.0 是目前最强的开放多模态视频生成模型——但写好
Seedance prompt 是一门**和 SD / Midjourney / Veo 完全不同的手艺**。
字节官方方法论有 1100 行。回报是真实的（10× 更好的输出），但门槛太高。

这个 skill 把那 1100 行 SOT 压缩成 Claude 驱动的工作流：

| 问题 | 修复 |
|---|---|
| `highly detailed` / `8k` / `masterpiece` 等 junk token 反而拉低输出 | Lint 抓出来，自动换成命名相机（Sony A7S3 / ARRI Alexa / iPhone 15 Pro / 35mm 胶片）|
| 时间轴 ≥5 段会被模型合并/丢失 | Skill 强制 3 段结构 |
| I2V 重述主体导致 identity drift | Skill 强制「只写动作不写主体」|
| R2V 只在首段绑定 `@图片1` 导致换脸 | Skill 每段重新绑定 |
| 多人用 `美女3` 这种写法被 parser 误读 | Skill 用字母 A/B/C |
| 模糊动作描述（"interacts with the product"）出来也模糊 | Skill 重写为具体动词序列（"rotates the bottle, places it on the table"）|

---

## 安装

### A. 一行安装

```bash
curl -fsSL https://raw.githubusercontent.com/scotti1i/seedance-superprompt/main/install.sh | bash
```

### B. 手动

```bash
git clone https://github.com/scotti1i/seedance-superprompt.git
cd seedance-superprompt && bash install.sh
```

### C. 软链（贡献者用）

```bash
git clone https://github.com/scotti1i/seedance-superprompt.git
ln -s "$(pwd)/seedance-superprompt/seedance" ~/.claude/skills/seedance
```

装完重启 Claude Code。技能名 `/seedance`，提到 Seedance / 视频生成 /
Dreamina / 即梦 时也会自动触发。

---

## 快速开始

### 🪄 修复一个烂 prompt（最常用）

```
/seedance fix 一个漂亮女生喝咖啡的视频，cinematic, 8k, masterpiece
```

→ 一次返回 lint 报告 + 完整重写 + diff 解释。

### 速通模式（一句话）

```
/seedance 10s 竖屏 一只猫在午后阳光下玩球
```

→ 自动选 T2V / 9:16 / 10s / seedance2.0fast，生成 3 段工程化 prompt。

### 精控模式（锁参数）

```
/seedance 产品展示 8s 16:9 电影感 ARRI Alexa 风格 不要字幕
```

→ Skill 100% 遵守你指定的每个约束。

### 用模板

```
/seedance 用 pet-vlog 模板，我家猫是英短，产品是粉色胸背带
```

→ 自动填模板里的变量。

### 只 lint 不重写

```
/seedance lint <你的 prompt>
```

→ 跑 25 条规则，给评分。不改写。

---

## 特性

### 🪄 一个 skill 三种工作流
- **WRITE**: 想法 → 工程化 prompt
- **LINT**: 审计任何 prompt，25 条 anti-pattern 规则
- **FIX**: 重写烂 prompt + 完整 diff 解释

### 🎯 自动选模式
根据你手上的素材自动选 T2V / I2V / R2V / V2V / Frames。

### 📐 方法论强制
25 条 lint 规则，整理自字节 / 火山引擎官方 prompting 指南 + 社区公开
field report。

### 📚 模板库
8 个验证过的生产模板：萌宠 vlog、产品展示、晨间日常、城市 walk、
美食 ASMR、古风短剧、开箱、化妆品对比。

### 🌍 多客户端输出
同一份 prompt 适用：
- [jimeng.jianying.com](https://jimeng.jianying.com)（国内即梦网页）
- [dreamina.capcut.com](https://dreamina.capcut.com)（国际版）
- 火山引擎 API（`doubao-seedance-1-0-pro-250528`）
- BytePlus ModelArk
- PiAPI / Coze / 即梦助手（第三方）
- 字节内部 dreamina CLI（如果你有权限）

### 🚫 零基础设施
零 API 调用。零凭证。零付费。纯 prompt 工程。

### 🌐 中英双语
prompt 支持中文、英文、中英混杂。工程标签默认英文（行业惯例），内容描述
随你。

---

## 仓库结构

```
seedance-superprompt/
├── README.md            ← 英文 README
├── README.zh-CN.md      ← 你在看的这份
├── LICENSE              ← MIT
├── CONTRIBUTING.md      ← 怎么贡献模板 / lint 规则
├── install.sh           ← 一行安装脚本
├── TESTING.md           ← 端到端测试场景
├── .github/             ← issue + PR 模板
└── seedance/            ← skill 本体（symlink 目标）
    ├── SKILL.md         ← Claude 入口
    ├── methodology.md   ← 500 行精简版 SOT
    ├── modes/           ← 5 个模式手册（t2v / i2v / r2v / v2v / frames）
    ├── decision-trees/  ← 4 棵决策树（模式选择 / 时长预算 / 相机选择 / 审核安全区）
    ├── templates/       ← 经过验证的生产模板
    ├── examples/        ← good / bad prompt 示例对照
    └── lint-rules.md    ← 25 条 anti-pattern 规则集
```

---

## 模板列表

| 模板 | 模式 | 时长 | 场景 |
|---|---|---|---|
| [pet-vlog](seedance/templates/pet-vlog.md) | R2V | 15s | 宠物户外 walk + 产品展示（抖音/小红书爆款格式）|
| [product-showcase](seedance/templates/product-showcase.md) | R2V | 10s | 产品 hero rotation + 卖点 + 品牌特写 |
| [morning-routine](seedance/templates/morning-routine.md) | T2V | 10s | 慢镜头电影感晨间日常 |
| [city-walk](seedance/templates/city-walk.md) | T2V | 10s | POV 城市探索 |
| [food-asmr](seedance/templates/food-asmr.md) | T2V | 8s | 美食特写 + ASMR 音效 |
| [ancient-drama](seedance/templates/ancient-drama.md) | R2V | 15s | 古风武侠双人对决 |
| [unboxing](seedance/templates/unboxing.md) | R2V | 10s | POV 开箱 |
| [before-after](seedance/templates/before-after.md) | R2V | 8s | 化妆品 / 变身 before-after |

提 PR 加你自己的——见 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 方法论摘要

Skill 强制执行：

1. **八要素公式** — 主体 + 动作 + 场景 + 光影 + 镜头 + 风格 + 画质 + 约束，按此顺序
2. **三段时间轴** — `[0-Xs]` `[X-Ys]` `[Y-Zs]`。≥5 段会塌缩
3. **场景三件套** — 每段都要 颜色 + 材质 + 光源
4. **一段一种运镜** — 不要叠加推+拉+摇+移
5. **命名相机替代 "cinematic"** — Sony A7S3、ARRI Alexa、iPhone 15 Pro、35mm 胶片
6. **R2V 每段重新 `@图片N` 绑定** — 不绑定就会换脸
7. **多人用字母 A/B/C** — 数字会让 parser 误读
8. **I2V 禁止重述主体** — 图已经包含信息了
9. **首行声明 Overall audio atmosphere** — 否则 BGM 被忽略
10. **3-5 个具体负面词** — 多了会 over-constrain

完整细节: [methodology.md](seedance/methodology.md)。

---

## 这个 skill **不是**什么

- ❌ dreamina / volcengine CLI 的包装
- ❌ 视频生成器（它产 prompt，不产视频）
- ❌ 付费服务（无 credits、无 API key）
- ❌ 保证书 — Seedance 本身是非确定性的，hero shot 一般要 2-5 次重试
- ❌ ByteDance / 火山引擎 / 即梦 / 剪映的官方关联

---

## 👤 关于作者

**ScottLi** ([@scotti1i](https://github.com/scotti1i)) — AI builder，
专注 TikTok Shop、跨境电商、AI 视频工具。
把自己日常用的方法论工具开源出来。

如果这个 skill 对你有帮助，请到 GitHub 给个 ⭐，下面任意渠道随时联系
——每条都看。

| 平台 | 账号 |
|---|---|
| 📧 邮箱 | [yifanlitju@gmail.com](mailto:yifanlitju@gmail.com) |
| ▶️ YouTube | [@ScottGlobalAI](https://www.youtube.com/@ScottGlobalAI) |
| 🎵 抖音 | [SCT出海Scott](https://www.douyin.com/user/MS4wLjABAAAAwB0Jkhs1X4lC6k2OhohpTuQgtxoZQiYHmGhqleB9kGQ) |
| 🐙 GitHub | [@scotti1i](https://github.com/scotti1i) |

### 合作 / 咨询

如果你的团队在大规模使用 Seedance 2.0 或字节其他视频模型，需要：
- 定制化 prompt 工程审计
- 方法论培训
- 工作流集成

欢迎通过上面任意渠道联系。

---

## 贡献

每次有人贡献，skill 都变得更强：

- 加新的验证过的模板
- 发现新的 anti-pattern 并写 lint 规则
- Seedance 模型版本更新后修方法论
- 贡献好/坏 prompt 案例

见 [CONTRIBUTING.md](CONTRIBUTING.md) 和
[issue 模板](.github/ISSUE_TEMPLATE/) 起步。

---

## 路线图

- [ ] 再加 12 个模板（目标：20 个）
- [ ] 跨模型适配器（当 Veo / Sora prompt 跟 Seedance 分叉时）
- [ ] 字节官方 SOT 更新自动同步
- [ ] 视频输出基准评测（带匿名化的评估样本）
- [ ] CLI 配套（lint 做成独立二进制）

---

## License

MIT. 见 [LICENSE](LICENSE)。

---

## 致谢

- 字节 Seedance 团队的官方 prompting 指南
- 火山引擎文档团队的 API 参考
- 在飞书 / 微信 / 小 Discord 里分享 field-tested prompt 的创作者社群——
  你们的 write-up 让这个项目成为可能
