# 推文推荐的 4 个设计资源调研（给 AI coding agent 的设计能力库）

> 调研日期 2026-07-07。所有事实带来源 URL。标「未验证」= 没实取到原文/没把握。
> 结论优先级：直接安装 > 拆迁内容 > 只借思路。

---

## 1. Leonxlnx/taste-skill — "Anti-Slop 前端框架"（4 个里最有价值）

**一句话定位**：一组可移植 Agent Skill（SKILL.md），核心是把 LLM 生成网页的"AI 味"（AI tells）逐条列成硬规则去消除，让 Codex/Cursor/Claude 产出 Awwwards 级前端而非模板脸。MIT 许可。
来源：https://github.com/Leonxlnx/taste-skill · 官网 https://tasteskill.dev

### 仓库结构（实测 git tree）
- `skills/` 下 13 个 skill，每个一个目录 + `SKILL.md`；另有 `skills/llms.txt`（skill 索引）、根 `skill.sh`（本地注册表脚本）、`CHANGELOG.md`、`research/`。
- 实际 skill 名与推文里说的不完全一致（推文用的是 install name，仓库用 folder name）。folder → install name 对照（来源 README + skill.sh）：
  - `taste-skill` → `design-taste-frontend`（v2 旗舰，87KB，1206 行，本仓核心资产）
  - `taste-skill-v1` → `design-taste-frontend-v1`（旧版保留，21KB）
  - `gpt-tasteskill` → `gpt-taste`（GPT/Codex 专用严格版，7.8KB）
  - `image-to-code-skill` → `image-to-code`（图→分析→码流水线，36KB）
  - `redesign-skill` → `redesign-existing-projects`（改造现有项目，15KB）
  - `soft-skill` → `high-end-visual-design`（高端柔性 UI，10.5KB）
  - `minimalist-skill` → `minimalist-ui`（Notion/Linear 编辑风，7.9KB）
  - `brutalist-skill` → `industrial-brutalist-ui`（Beta，8.4KB）
  - `output-skill` → `full-output-enforcement`（防偷懒/占位符，2.6KB）
  - `stitch-skill` → `stitch-design-taste`（Google Stitch 兼容 + DESIGN.md 导出，11.8KB）
  - 纯图像生成（不出码）：`imagegen-frontend-web`(36KB) / `imagegen-frontend-mobile`(40KB) / `brandkit`(16KB)

### 核心方法论（附旗舰 taste-skill/SKILL.md 原文证据）
方法论骨架（14 节 + 附录 A/B/C，来源 taste-v2 章节头）：
1. **Brief Inference（先读需求再动手）**：强制先输出一行 "Design Read"：`"Reading this as: <page kind> for <audience>, with a <vibe> language, leaning toward <design system>"`。模糊时只问一个问题，不许静默默认。
2. **三个刻度盘**：`DESIGN_VARIANCE / MOTION_INTENSITY / VISUAL_DENSITY`（1-10，基线 8/6/4）。给了"信号词→刻度值"推断表和用例预设表（landing/portfolio/public-sector 各一套）。
3. **Brief → Design System Map**：需求命中企业系 UI 就装官方包（Fluent/Material 3/Carbon/Polaris/Atlaskit/Primer/GOV.UK/USWDS/Radix/shadcn/Tailwind），并有"诚实规则"：不许手搓官方系统的 CSS、不许导入 token 再覆盖 90%、一个项目只用一套系统。
4. **默认技术栈约定**：React/Next RSC、Tailwind v4、Motion(`motion/react`)、`next/font`；图标只许 Phosphor/HugeIcons/Radix/Tabler（点名 discouraged `lucide`）。
5. **Design Engineering Directives（偏见纠正，最密的干货）**：逐条列 AI 默认毛病 + override。原文摘录（可直接引用）：
   > "THE LILA RULE: The 'AI Purple / Blue glow' aesthetic is discouraged as a default."
   > "SERIF DISCIPLINE ... 'creative brief = serif' is the single most-tested AI tell in production rounds. Specifically BANNED as defaults: Fraunces and Instrument_Serif."
   > "PREMIUM-CONSUMER PALETTE BAN" —直接把 AI 惯用的 beige+brass+oxblood+espresso **具体 hex** 全列出来禁掉（`#f5f1ea`/`#b08947`/`#1a1714`…），给 7 套替代配色并要求"上一个 premium 项目用过就不许再用"。
   > "EYEBROW RESTRAINT (the #1 violated rule) ... Maximum 1 eyebrow per 3 sections ... Pre-Flight Check is mechanical: count instances of `uppercase tracking`."
6. **GSAP 权威代码骨架（5.A/5.B/5.C）**：Sticky-Stack、Horizontal-Pan、Scroll-Reveal 三个可直接抄的 `tsx`，都带 `useReducedMotion` + `gsap.context()` cleanup，且点名常见 bug 修法（`start:"top top"` 而非 `"top center"`）。5.D 明确硬禁 `window.addEventListener("scroll")`。
7. **AI Tells 禁令表（第 9 节）**：视觉/排版/布局/内容/组件五类；9.F "Production-Test Tells" 是从真实测试里抠出来的（禁 hero 里的 `V0.6/BETA`、禁 `00/INDEX` 段号眉标、禁 `·` 滥用、禁 div 假截图、禁 "Quietly trusted by" 文案…）；9.G **EM-DASH 全面禁令**（二元：`—`/`–` 出现一次即 Pre-Flight fail）。
8. **第 14 节 Final Pre-Flight Check**：~60 个 checkbox 的机械验收清单，"任一勾不上即未完成"。这是整份 skill 的"验收内核"，可独立拆迁。

### 质量判断
- 高质量、实战沉淀重（规则大量写着 "the #1 violated rule in production tests"），不是空泛美学口号而是**可机械检查的 fail 条件**。有真赞助方（Vercel OSS Program、animations.dev）背书。缺点：旗舰 v2 明确自称 "experimental / 迭代中"，87KB 单文件偏大（一次性全量注入吃 token）；部分规则强绑 React/Next/Tailwind/Motion 技术栈，非该栈需裁剪。
- `gpt-taste`（紧凑版）另有巧思：用"模拟 Python RNG（prompt 字符数取模）做 `random.choice`"强制打破"总选第一个布局"的惰性 + AIDA 结构 + `<design_plan>` 前置验收。来源 gpt-taste.md。

### 对我们系统的可用形态：**直接安装 + 重度拆迁**（最高优先）
- 直接安装（框架无关，四大 agent 通用）：
  `npx skills add https://github.com/Leonxlnx/taste-skill`
  单装：`npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"`
- 拆迁（MIT 允许，建议）：把**第 9 节 AI Tells + 第 14 节 Pre-Flight Check + 第 4 节 Directives**抽成我们自己的"验收 skill/规则文件"，作为所有出码 agent 的强制后置门。GSAP 5.A/5.B/5.C 三段骨架可直接进我们的 block 库。
- 只借思路：三刻度盘 + Design Read 一行式，值得并进我们的需求澄清环节。

---

## 2. Paidax01/web-to-design-md — 网页 → DESIGN.md 提取器

**一句话定位**：一个 SKILL.md 型工具，用 `agent-browser` 深挖任意线上网页（DOM/computed style/CSS 变量/交互态），产出一份可复用的 Stitch 风 `DESIGN.md` + 配套 HTML 预览板，供另一个 agent"无需看原站即可复刻设计语言"。
来源：https://github.com/Paidax01/web-to-design-md · 许可未选（README "Publishing Notes" 明说发布前要补 license，**未验证有正式 LICENSE 文件**，拆迁需谨慎）

### 仓库结构（实测）
```
SKILL.md (19KB/384 行)  README.md
agents/openai.yaml (4 行)
assets/  DESIGN.template.md (275 行)  design-preview-shell.template.html
references/  browser-tooling-bootstrap.md (77)  website-reading-checklist.md (244)
scripts/  check-browser-tooling.mjs  extract-browser-evidence.mjs (883 行)  render-design-preview.mjs
```

### 工作原理（怎么提取颜色/字体/间距/组件，来源 SKILL.md 原文）
- **不走截图优先，走"browser-eval + evidence-first"**：用 `agent-browser open/wait/eval` 抓：rendered HTML、代表节点 outerHTML、`:root` CSS 变量、可读 stylesheet 规则、headings/buttons/cards/nav 的 computed style、可见文案/CTA、hover/active/sticky/expanded 交互态。截图只作最后交叉核对。原文明令："Do not replace steps 4 through 8 with screenshots."
- **6 个提取 pass**：Scope → Baseline（desktop+tablet+mobile，先慢滚全页触发懒加载）→ Design System（颜色角色/明暗双主题/字阶/间距节律/圆角/边框/阴影/图像/图标/动效/密度）→ Components & States → Interaction Behavior → Content & Brand Voice。支持 **明暗主题 sweep**（触发切换、分别取证、记录哪些 token 反转）。
- **输出契约**：按 `assets/DESIGN.template.md` 固定 9 节结构（Visual Theme / Color Palette & Roles / Typography / Component Stylings / Layout Principles / Depth & Elevation / Do's & Don'ts / Responsive / **Agent Prompt Guide**）+ 可选附录；HTML 预览用固定 shell 模板填 token。
- **合成规则/质量门**：语义名优先于原始 token、颜色带精确 hex 且绑功能角色、区分"观察事实 vs 推断"、末尾自检"另一个 agent 能否仅凭此文档复刻整站气质"。

### 输出格式样例（DESIGN.template.md 骨架 + SKILL 里的 good/bad 对照）
> Good: "Primary actions use a cool electric blue `#3B82F6` on dark charcoal surfaces."
> Bad: "Button background is `rgb(59, 130, 246)`."（只有数值、无角色）
（template 275 行含 Key Characteristics / Primary·Neutral Scale·Surface / Font Family·Hierarchy / Buttons·Cards·Inputs·Nav / Spacing·Grid·Radius / Breakpoints·Touch·Collapsing / Example Component Prompts·Iteration Guide 等子节。）

### 局限性
- **硬依赖 `agent-browser` 这个特定运行时**：SKILL.md 反复强调不许静默 fallback 到 Playwright/Chrome CLI；缺它就要先装。我们环境里若没有 `agent-browser`，需先解决工具链（**未验证 agent-browser 在本机可用**）。
- 许可缺失（见上），直接把它的 template/script 并入我们仓库前需确认授权。
- 定位是"文档化"而非"复刻成码"，与 taste-skill（出码）互补而非重叠。

### 对我们系统的可用形态：**拆思路 + 拆模板（授权待确认）**
- 最值钱的是 **DESIGN.template.md 的 9 节结构 + good/bad 合成规则 + browser-eval 提取清单**：正好补齐我们"给一个参考站→自动产出设计规范喂给出码 agent"的缺口。
- 建议：先借 `DESIGN.md` 结构和"evidence-first、区分观察/推断、颜色绑角色"的方法论；模板文件的直接复制等确认 license。工具链上，我们已有内置浏览器（Playwright Chromium，见项目铁律6），可用它替代 `agent-browser` 复刻同套 eval 提取逻辑。

---

## 3. greensock/gsap-skills — GSAP 官方 Agent Skills

**一句话定位**：GreenSock 官方出的 8 个 GSAP Agent Skill，教 agent 正确用 GSAP（core/timeline/ScrollTrigger/plugins/utils/react/performance/frameworks），Agent Skills 标准格式，`npx skills` 一键装 40+ agent。MIT。
来源：https://github.com/greensock/gsap-skills · 关键背书：README 明说 **Webflow 收购 GSAP 后全部插件免费（含 SplitText/MorphSVG），无需 Club 会员/auth token/私有 registry**。

### 包含哪些 skill（8 个，来源 README + llms.txt）
| skill | 覆盖 |
|---|---|
| gsap-core | `to/from/fromTo`、easing、duration、stagger、defaults、autoAlpha、`matchMedia`（响应式+`prefers-reduced-motion`）|
| gsap-timeline | timeline、position 参数、labels、嵌套、playback |
| **gsap-scrolltrigger** | 滚动联动、pin、scrub、triggers、refresh、cleanup |
| gsap-plugins | ScrollTo/ScrollSmoother/Flip/Draggable/Inertia/Observer/**SplitText**/ScrambleText/SVG/物理/CustomEase/GSDevTools |
| gsap-utils | clamp/mapRange/normalize/interpolate/random/snap/toArray/wrap/pipe |
| gsap-react | `useGSAP` hook、refs、`gsap.context()`、cleanup、SSR |
| gsap-performance | transform 优先、will-change、batching、ScrollTrigger tips |
| gsap-frameworks | Vue/Svelte/Nuxt/SvelteKit 生命周期与 cleanup |
- 额外资产：`examples/`（vanilla/react/vue/nuxt 可跑 demo）、`.github/copilot-instructions.md` + path-specific 指令（覆盖 Copilot）、`.claude-plugin/` + `.cursor-plugin/` 配置、`CLAUDE.md`/`GEMINI.md`/`AGENTS.md`。

### 覆盖的动效场景（ScrollTrigger/SplitText/Flip 等）
- ScrollTrigger 覆盖完整（实读 SKILL.md 证据）：start/end 语义、`scrub`、`toggleActions`、`pin`/`pinSpacing`、`containerAnimation`（假横向滚动）、`snap`、`ScrollTrigger.batch()`（替代 IntersectionObserver）、`refreshPriority`、`clamp()`(v3.12+)、全套回调、production 去 `markers`。
- Flip / Draggable / SplitText / MorphSVG / ScrollSmoother 归在 gsap-plugins；React 清理归 gsap-react（`useGSAP` + `ctx.revert()`）。

### 怎么安装（Claude Code / Codex）
- 推荐：`npx skills add https://github.com/greensock/gsap-skills`（自动识别 agent）
- Claude Code：`/plugin marketplace add greensock/gsap-skills`
- Codex：copy `skills/` 到 `~/.codex/skills/`（README 给了全 agent 目录对照表）

### 质量如何
- **官方出品、质量高**：ScrollTrigger skill 的 API 表精确到 `refreshPriority`/`containerAnimation`/`clamp()` 这种易错细节，明显是懂行的人写的；每个 skill 带 frontmatter trigger 词 + "related skills" 交叉引用；risk level 标 LOW。是四个资源里工程严谨度最高的。

### 对我们系统的可用形态：**直接安装**（强烈推荐，零改造）
- 直接 `npx skills add https://github.com/greensock/gsap-skills` 进 Codex/Claude Code。它和 taste-skill 是天然搭档：taste-skill 决定"何时/为何用动效 + 3 段场景骨架"，gsap-skills 供"正确的 GSAP API 细节 + cleanup"。
- 只借思路：其 `examples/` 四栈 demo 可作我们 block 库的动效参考实现。

---

## 4. DavidHDev/react-bits (reactbits.dev) — 动画 React 组件库

**一句话定位**：最大的动画 React 组件开源库，130+（实测 134）个可高度定制的文字动效/背景/交互组件，每组件 4 变体（JS/TS × CSS/Tailwind），copy-paste 或 CLI 装，42.9k star。**MIT + Commons Clause**（可商用于产品，但**禁止再分发/转售/re-bundle 组件本身**）。
来源：https://github.com/DavidHDev/react-bits · https://reactbits.dev

### 组件分类 + 数量（实测 git API 计数）
| 分类 | 数量 | 样例 |
|---|---|---|
| TextAnimations | 23 | SplitText, BlurText, DecryptedText, ScrambledText, GradientText, ShinyText, TextPressure, VariableProximity, CircularText, CountUp |
| Animations | 30 | （交互/微动效）|
| Backgrounds | 45 | （动画背景，最大类）|
| Components | 36 | （UI 组件）|
| **合计** | **134** | README 写 "130+ growing weekly" |

### 技术栈变体 / 规模 / 许可 / agent 怎么用
- **4 变体/组件**：JS-CSS、JS-TW(Tailwind)、TS-CSS、TS-TW。仓库 `src/` 下有 `ts-default`/`ts-tailwind`/`tailwind` 等目录分别落地。轻量、tree-shakeable、最小依赖。
- **安装**：支持 shadcn 与 jsrepo 两种 CLI。例：`npx shadcn@latest add @react-bits/BlurText-TS-TW`（每个组件页给现成命令）。
- **agent 消费方式**：**有官方 MCP server**（reactbits.dev 文档站有 MCP 配置指南，让 AI agent 直接用库）——注意：**无 llms.txt**（实测 reactbits.dev/llms.txt 无该索引；仓库内是 `.context/new-component.md` 这种贡献者模板，非 agent 索引）。
- 额外：官网带 3 个创作工具（Background Studio 导出背景为 video/image/code、Shape Magic 导出 SVG/clip-path、Texture Lab 20+ 图像效果）。

### 质量
- 头部项目、star 42.9k、周更、有专门 MCP + shadcn/jsrepo registry 分发，工程成熟度高。**唯一硬约束是许可**：Commons Clause 明文——"you do not sell, sublicense, or redistribute the components themselves—whether alone, in a bundle, or as a ported version."

### 对我们系统的可用形态：**产品内使用 OK，禁止拆迁进我们的库分发**
- 合规用法：让我们的 agent 通过**官方 MCP** 或 shadcn/jsrepo CLI 把单个组件装进**最终用户产品**里（这是"as part of an application/product"，许可允许）。
- **禁止**：把 react-bits 组件源码复制进我们自己的 block 库/skill 包再随产品分发（=redistribute/re-bundle，违反 Commons Clause）。
- 推荐动作：把 react-bits 定位成"运行时组件供给源"（经 MCP/CLI 按需拉取），而非"可拆迁素材"。它的**组件命名表（134 个动效名）**倒是可无风险地作为 taste-skill "Reference Vocabulary" 的补充词表，帮 agent 知道"有哪些动效可点名要"。

---

## 横向定位（四者如何拼成一套能力库）
- **taste-skill** = 审美决策 + 机械验收（大脑/质检）→ 直接装 + 拆 Pre-Flight/AI-Tells。
- **web-to-design-md** = 参考站→设计规范提取（输入端）→ 借结构与方法，工具链换成我们内置 Chromium。
- **gsap-skills** = 动效正确 API（手）→ 直接装，与 taste-skill 骨架互补。
- **react-bits** = 现成动效组件（零件库）→ 经 MCP/CLI 供给产品，勿拆迁分发。

