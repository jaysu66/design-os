# ENGINE — 参考检索引擎(三来源优先级)

> 用途:UI 任务需要「长什么样」的参考时,按本文件优先级检索,产出候选给用户挑。
> 来源:research/04-reference-ecosystem.md + 01-tweet-resources.md(2026-07-07)。

## 优先级总则

**PERSONAL 个人精选 > 本地风格档案库(可选,自备) > 外部实时源。**
前两级是本地结构化数据(DESIGN.md + tokens,零网络、零墙);外部实时只在本地打不中时上。

## 来源 1:PERSONAL 个人精选库(权重最高)

- 什么时候:永远先查。用户亲手收藏的站 = 已表达过的审美偏好,压过一切外部参考。
- 怎么用:Glob `3-references/PERSONAL/*/DESIGN.md`(相对系统根),按站名/日期扫;每条目含 DESIGN.md(9 节设计语言)+ screenshot.png。入库协议见 EXTRACT.md。
- 注意:空库是常态(新系统);为空就降到来源 2,并顺口提示用户「丢链接进来即可收藏」。

## 来源 2:本地风格档案库(可选,自备)

- 什么时候:PERSONAL 无命中,需要「像某产品/某气质」的成套 design tokens(Vercel / Linear / Stripe / 编辑风 / 暗色 dashboard / fintech / AI app…)。
- **这是可选外部依赖**:design-os 本身**不自带风格库**(第三方设计资产有版权,不随本仓库分发)。你可以挂接任意符合下面契约的本地库,或跳过本级直接用来源 3。
- **挂接契约**:一个目录 = 一个风格;每个风格目录至少含 `DESIGN.md`(设计语言,格式见 EXTRACT.md 的 9 节),可选 `design-tokens.json` / `tailwind.css` / `css-variables.css` / `screenshot.png`。库根用环境变量 `DESIGN_OS_STYLE_LIB` 指定。
- **怎么检索**:库若自带检索脚本(如按气质 search / brief / 载入 tokens)就用它;否则 `Glob $DESIGN_OS_STYLE_LIB/*/DESIGN.md` 扫目录名 + 读 DESIGN.md 头部气质描述定位,写码要 tokens 时再载入完整档案。
- **怎么自建**:见 EXTRACT.md —— 用户丢链接即可把任意公开站提取成一个符合契约的档案;`3-references/PERSONAL/` 就是你自建库的默认落点(收藏久了它本身就是你的私有风格库)。
- 注意:1 主风格 + ≤1 辅风格,混 3 个以上必出泥;adapt 不 clone(不搬 logo/品牌文案/商标构图)。

### 从哪弄一个风格库

- **自建(最干净)**:用 EXTRACT.md 从「主动公开设计规范」的站提取——Material Design / Ant Design / shadcn / Vercel(Geist) 等,落进 PERSONAL。
- **用现成产品**:styles.refero.design 这类站提供成套结构化设计 tokens。**按其官方条款订阅 / 导出**后,把数据放进 `DESIGN_OS_STYLE_LIB` 指向的目录即可挂接。design-os 只定义「怎么挂接和检索」,**不替你获取、不分发任何第三方数据**。

## 来源 3:外部实时(本地打不中才上)

| 源 | 什么时候用 | 怎么用 | 注意什么 |
|---|---|---|---|
| 21st.dev | 要成段业务组件代码(hero/pricing/testimonials 等 30+ 类) | Magic MCP `21st.dev/api/mcp`(需 free API key,免费计划 10 credits/mo + 无限 UI inspirations);或直接浏览 <https://21st.dev/>(llms.txt 完整,sitemap 9696) | 组件是社区 UGC,逐个看 license |
| uiverse.io | 要原子微组件(button/loader/toggle/checkbox)的纯 HTML/CSS | 站上直接复制代码 <https://uiverse.io/> | 全免费开源(MIT/CC),授权最干净 |
| SiteInspire | 要「整站视觉调性」策展参考(按 style/type/subject 检索) | 官方 MCP `https://siteinspire.com/api/mcp`(streamable-http,匿名可调);7 工具:search_sites / list_websites / all_categories / get_website / list_profiles / get_profile / popular_websites | 产出只有截图 URL + 外链,无 tokens/无代码 |
| react-bits | 要现成动效 React 组件(134 个:Text 23 / Animations 30 / Backgrounds 45 / Components 36) | 官方 MCP(reactbits.dev 有配置指南)或 `npx shadcn@latest add @react-bits/<组件名>-TS-TW` <https://reactbits.dev> | **MIT + Commons Clause:装进最终用户产品 OK;禁止把组件源码拆进自家库/skill 包再分发或转售**。组件命名表可当「动效词汇表」无风险引用 |
| recent.design(=godly.website,已 301) | 要动效/motion 视觉参考流(视频+图片,免费无墙) | 浏览 <https://recent.design/>;CDN 直链规整:`cdn.recent.design/items/{id}/0/{small|medium|large}.jpg` + `poster.jpg` | **robots 标 `ai-train=no, use=reference`:只可参考,禁训练用途** |
| 用户丢的链接 | 用户明确给了参考站/说「存进设计库」 | 走 EXTRACT.md 协议提取,落盘 PERSONAL | 版权边界见 EXTRACT.md |

其余纯截图库(Mobbin / Awwwards / Land-book / Lapa Ninja / Page Flows 等):付费墙或 Cloudflare 硬挡,不接程序化;需要时人逛 → 喂截图。

## 外挂 skill(能力增强,不是参考源)

| skill | 安装 | 分工 |
|---|---|---|
| taste-skill | `npx skills add https://github.com/Leonxlnx/taste-skill`(单装旗舰:`npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"`) | **审美与质检**:Design Read 一行式、三刻度盘(VARIANCE/MOTION/DENSITY)、AI-Tells 禁令表、~60 项 Pre-Flight 机械验收 |
| gsap-skills | `npx skills add https://github.com/greensock/gsap-skills`(Claude Code 亦可 `/plugin marketplace add greensock/gsap-skills`) | **动效 API 正确性**:GSAP 官方 8 skill(core/timeline/ScrollTrigger/plugins/utils/react/performance/frameworks),cleanup 与易错细节 |

搭配逻辑:taste 决定「何时/为何用动效 + 场景骨架」,gsap 供「正确的 API 细节 + cleanup」。

## 「出 3 候选」标准动作(检索的默认输出)

1. 从需求提炼项目气质关键词(行业 + 情绪 + 密度,如「fintech 冷静 数据密」),按上面优先级逐级检索。
2. 凑满 3 个候选,每个一行:**[名字 + 主题(亮/暗) + 一句话气质 + 来源路径(PERSONAL 目录 / 档案库风格名 / URL)]**。
3. 停下,等用户挑一个(或都不要再换一批);**绝不替用户选**,选完才进实现。
