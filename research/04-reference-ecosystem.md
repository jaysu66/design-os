# 网页设计参考/灵感库生态盘点 —— 「能否变成 agent 弹药」评估

调研日期:2026-07-07。基线参照:**styles.refero.design**(1289 风格,每个含 DESIGN.md + design tokens + Tailwind 变量)是「结构化、可程序化、可离线检索」的公开风格库范例。本报告评估其他库有没有同样「结构化、可程序化、可离线检索」的价值。

**核心判据**:agent 要的不是"好看的截图",而是**结构化、可复制、可离线检索的设计信号**(tokens / 代码 / 稳定 id / 分类)。据此把所有库分成三档:
- **S 档(像 refero 一样值得归档)**:有可提取的结构化数据(代码/tokens),或有官方 MCP/API 直接喂给 agent。
- **A 档(agent 能用,但不归档)**:有官方 MCP/API,实时查即可,无需落盘(数据是截图,离线无意义)。
- **B/C 档(基本喂不动)**:纯截图/视频 + 登录墙/Cloudflare,agent 只能"人逛→喂截图"。

> 关键区分:**styles.refero.design ≠ refero.design**。本体 refero.design 是截图库(同 Mobbin);styles 子站是它衍生的「风格/tokens」产品,数据结构完全不同 —— 这正是它能提取 tokens 的原因。

---

## 关键发现(先说结论性的)

1. **SiteInspire 有官方公开 MCP**(`https://siteinspire.com/api/mcp`,streamable-http,7 工具,**匿名可调、无需 key**)—— agent 友好度天花板级,但产出只有截图 URL + 外链,**无 tokens/无代码**。来源:实测 robots + `initialize` 返回 200。
2. **Mobbin 有官方 MCP**(621,500 截图),但 **MCP 仅付费计划**,免费墙极严(4 app / 10 截图),内容纯截图。
3. **21st.dev 是"代码级"库里对 AI 最友好的**:免费计划就含 Magic MCP + 无限 UI inspirations,组件是 **React/Next/Tailwind 可复制代码 + SVG logo 可导 JSX/TSX**,有 llms.txt + sitemap(9696 条)+ ClaudeBot 白名单。
4. **Godly = Recent.design(已改名合并)**:`godly.website` 301→`recent.design`。视频/图片流,CDN 直链规整,有稳定 id,免费无墙。
5. **Land-book / Lapa Ninja = Cloudflare 强挡**,脚本直取被 JS challenge 拦;Page Flows 全付费墙 + 视频录屏。
6. **D 组新形态**:Figma 已上原生 MCP(发 CSS/字体/色板/截图给模型);出现 "设计规范即 MCP"(如 Apple HIG Doctor CLI+MCP)。这类比"爬截图库"更贴近 refero 的价值。

---

## A 组(推文推荐)

### Mobbin — mobbin.com
- **内容类型**:真实 app/web UI 截图 + 完整 flows(流程截图序列)+ screen patterns。无代码、无 tokens。
- **覆盖/规模**:621,500+ 截图,mobile + web;分类体系极细(app category × flow × screen pattern 三维,llms.txt 列出数百类)。
- **付费墙**:极严。免费仅 4 最新 app / 4 最新 site / ~10 截图,search/filter/collection/export 全锁。付费 Starter $20、Pro $40/seat/mo(年付)。
- **程序化**:有 sitemap(2491,多为壳页)+ llms.txt(200)+ **官方 MCP,但仅付费计划含**(免费不给)。robots 明确 `GPTBot Disallow`。页面实测含 6× "Paywall" 字样。
- **结论**:**不值得归档**(内容是截图,离线无 tokens 意义;且付费墙+反爬)。**agent 用法**:若用户有付费账号→接官方 MCP 实时查(A 档);否则人逛→喂截图。
- 来源:robots.txt;sitemap.xml;llms.txt;[Pricing](https://mobbin.com/pricing);[Mobbin MCP 621,500](https://chatforest.com/reviews/mobbin-mcp-server/);[免费墙评测](https://coolcuration.com/mobbin-review-is-it-worth-it)

### Page Flows — pageflows.com
- **内容类型**:**录屏视频**(真实产品 user flow)+ step 注释 + app 截图。核心是视频。
- **覆盖/规模**:79,000+ app 截图,20,000+ apps,数百录制 flow。
- **付费墙**:全站付费。3天试用 $2.95,年费 $8.25/mo,Team $199/年3座。
- **程序化**:sitemap 几乎空(page-sitemap 仅 1、image 仅 4 条 → 真内容在登录后)。robots Disallow `/search`。视频 + 登录墙 → 极难。
- **结论**:**不值得归档**(视频形态本就喂不进文本 agent,又全付费)。**agent 用法**:人工看流程→口述/截关键帧喂模型。
- 来源:robots.txt;sitemap.xml(实测 loc≈1);[Pricing $8.25/mo](https://pageflows.com/pricing/)

### Recent.design(= Godly,已合并)— recent.design / godly.website
- **内容类型**:设计作品**视频流 + 图片**(Dribbble 风,但含大量 hover/motion 视频)。分类 Web/Interface/Branding/Product/Typography/Motion/3D 等。另有 Websites / OG Images / App Screenshots / App Icons 子板块。
- **覆盖/规模**:大(首屏即数十项瀑布流,持续更新);免费无墙。
- **付费墙**:**无登录墙**,免费浏览;仅有邮件订阅 + 赞助位。
- **程序化**:SPA(TanStack Start)。**数据 API = 独立域 `api.recent.design` + `/_serverFn/` RPC**(实测域名活但根 404,serverFn 需 HTML 请求上下文,**确切路由未从 bundle 提取到 → 未验证**)。**但 CDN 直链完全规整**:`cdn.recent.design/items/{id}/0/{small|medium|large}.jpg` + `poster.jpg`,详情页 id 稳定(`/i/{id}-slug`)。robots:`ai-train=no, use=reference`(明确不许训练,允许"参考"用途)。站内已有 `/skills` `/tools` 导航(自己在做 agent 方向)。
- **结论**:**半值得**——它是 A 组里唯一"结构化 id + 免费 + 无墙"的,但**内容是视频/图片,无 tokens/无代码**,归档下来 agent 也只能当"视觉参考图库",价值远低于 refero styles。**若要归档**:走 CDN 直链批量拉 poster/large.jpg + 详情页元数据(标题/分类),但注意 `ai-train=no` 红线——只可做"检索+喂截图给 agent 看"(reference 用途),不可拿去训练。
- 来源:home HTML(bundle 指向 `api.recent.design` + `/_serverFn/`);godly.website 实测 301→recent.design;robots.txt(Content-Signal `ai-train=no`)

### Collect UI — collectui.com
- **内容类型**:daily UI 挑战截图集(Dribbble shots 聚合)。纯截图,无代码/tokens。
- **覆盖/规模**:中,按 UI 元素/场景分类(daily challenge 归档)。
- **付费墙**:无(免费浏览)。
- **程序化**:robots 用 Cloudflare Content-Signal(`ai-train=no, use=reference`),未见 API。内容是外链 Dribbble 图。
- **结论**:**不值得归档**。价值低于 Recent(Collect 只是 Dribbble 二次聚合)。agent 用法:需要某类 UI 元素灵感时人逛→喂截图。
- 来源:robots.txt(实测)

### Minimal Gallery — minimal.gallery
- **内容类型**:极简风网站截图 + 外链。纯截图。
- **规模**:小而精(策展"minimal"单一风格)。
- **付费墙**:无。robots 为空(实测 200 但无内容)。
- **程序化**:无 API/sitemap 迹象;规模小。
- **结论**:**不值得归档**(样本量太小,单一风格)。agent 用法:找"极简"参考时人逛。
- 来源:robots.txt(实测空)

### Land-book — land-book.com
- **内容类型**:策展**落地页**截图大库 + 外链。截图为主。
- **规模**:大(落地页专门库,多年策展)。
- **付费墙**:部分 Pro(`/become-pro/`)。
- **程序化**:**Cloudflare 强挑战**(实测 sitemap.xml 直取被 JS challenge 拦)。robots **明确 Disallow `/api/`** + 几乎所有查询参数(search/sort/color/style/industry 全禁爬)。红线清晰:**站方明确不欢迎参数化爬取**。
- **结论**:**不值得归档**(反爬硬 + ToS 明示禁止参数爬 + 纯截图)。agent 用法:人逛→喂截图。
- 来源:robots.txt(大量 Disallow);sitemap 实测 Cloudflare challenge 页

### Lapa Ninja — lapa.ninja
- **内容类型**:落地页截图库(全页长截图)+ 646+ 免费设计资源下载。
- **规模**:7,300+ 落地页,15,000+ 全页截图。
- **付费墙**:大量免费。
- **程序化**:**Cloudflare 直接 403 挡 curl**(实测 robots/llms 均被拦)。有第三方镜像 `no-ads--lapaninja.netlify.app`(**未验证**是否官方/长期)。
- **结论**:**不值得归档**(Cloudflare 硬挡 + 纯截图)。agent 用法:人逛→喂截图;或用浏览器(真 Chrome)绕挑战单张取。
- 来源:实测 403;[Lapa 7,300+](https://www.lapa.ninja/);[Cloudflare 挡爬](https://automatio.ai/how-to-scrape/lapa-ninja)

---

## B 组(行业主流)

### Awwwards — awwwards.com
- **内容类型**:获奖网站截图 + 评分 + 外链。截图为主,无代码/tokens。
- **规模**:极大(多年获奖库)。
- **付费墙**:浏览免费,Pro 会员/评委功能付费。
- **程序化**:robots 大量 Disallow(gallery/search/vote 全禁)。**sitemap.xml 实测返回 HTML 而非 XML**(SPA 壳/挑战)→ sitemap 路线不通。
- **结论**:**不值得归档**(纯截图 + sitemap 不可用 + 大量禁爬路径)。agent 用法:看趋势时人逛→喂截图。
- 来源:robots.txt;sitemap.xml 实测非 XML

### Godly — godly.website
见 A 组 **Recent.design**(godly.website 301→recent.design,同一实体)。

### SiteInspire — siteinspire.com ⭐
- **内容类型**:策展网站截图 + taxonomy(style/type/subject)+ 外链 + 设计师 profile。**无代码/无 tokens**(MCP 明说 public 结果只给截图 URL + 外链)。
- **规模**:中大(多年策展,细分 taxonomy)。
- **付费墙**:浏览免费。
- **程序化**:**独一档 —— 官方公开 MCP `https://siteinspire.com/api/mcp`(streamable-http,匿名可调,实测 initialize 200)**。7 工具:`search_sites` / `list_websites` / `all_categories` / `get_website` / `list_profiles` / `get_profile` / `popular_websites`。robots 白名单 Claude-User/Claude-SearchBot(但黑名单 ClaudeBot 训练爬虫)。
- **结论**:**不归档、但 agent 直接用**(A 档标杆)。数据是截图 URL,离线无意义;有官方 MCP,**直接挂进 agent 实时查**即可(free-code 支持 MCP,可一行接入)。这是"参考类"库里 agent 接入成本最低的。
- 来源:robots.txt(`Allow /api/mcp`);MCP `initialize`/`tools/list` 实测 200 + 完整 tool schema

### Curated.design — curated.design
- **内容类型**:设计资源/灵感策展(实为 craftwork.design 旗下,robots Host 指向 craftwork.design)。偏资源合集 + 素材售卖。
- **规模**:中。
- **付费墙**:混合(部分素材付费)。
- **程序化**:robots Allow + 有 sitemap(craftwork.design/sitemap.xml),但**内容是外链合集/素材,非结构化设计数据**。
- **结论**:**不值得归档**(定位是素材导购,非"设计信号库")。agent 用法:找素材/工具时人逛。
- 来源:robots.txt(Host=craftwork.design)

### Dark Mode Design — darkmode.design
- **内容类型**:暗色模式网站截图集 + 外链。纯截图,单一主题。
- **规模**:小-中(单一"dark mode"策展)。
- **付费墙**:无。robots 实测空/不可达(000)。
- **程序化**:无 API/sitemap 迹象;站点探查连接失败(**未验证**是否长期在线)。
- **结论**:**不值得归档**(样本单一 + 站点稳定性存疑)。agent 用法:找暗色参考时人逛。
- 来源:robots 实测 000(连接失败)

### SaaS Landing Page — saaslandingpage.com
- **内容类型**:SaaS 落地页截图 + **template**(部分含设计拆解/文章)。截图 + 编辑文章,无 tokens。
- **规模**:中(post-sitemap 947,template-sitemap 87)。
- **付费墙**:浏览免费(WordPress 站)。
- **程序化**:WordPress + Yoast sitemap(结构清晰:post/page/template/article/category/tag 分表)。robots `ai-train=no, use=reference`。可爬性中等(WP 结构好爬,但内容是截图+文章)。
- **结论**:**边缘**——SaaS 落地页文案/结构对"营销智能体"有参考价值,但**是截图+文章非 tokens**。若用户做 SaaS 落地页方向,可轻量归档 template/article 的文本(标题+拆解),但价值远低于 refero。agent 用法:优先人逛喂截图;文本部分可选择性爬 WP sitemap。
- 来源:robots.txt;sitemap 分表(post 947/template 87 实测)

### Refero(本体)— refero.design
- **内容类型**:UI/UX 参考**截图库**(web + iOS),高级搜索。本体是截图(同 Mobbin);**tokens/代码在衍生的 styles 子站**。
- **规模**:自称"最大 UI/UX 参考库,数万张截图"(页面 SPA 壳,数字 JS 渲染)。
- **付费墙**:有 sign in(登录门槛,细节未验证)。
- **程序化**:SPA 壳(4.5KB);本体未见公开 API。**styles.refero.design 子站**是它衍生的结构化风格/tokens 产品(数据结构不同)。
- **结论**:本体**不值得再归档**(截图库,同 Mobbin 定位,tokens 不在本体)。真正有价值的是衍生的 styles 子站(结构化 tokens)。
- 来源:refero.design meta description + HTML 实测(SPA 壳);robots.txt

---

## C 组(组件/代码级)

### 21st.dev — 21st.dev ⭐⭐
- **内容类型**:**可复制 React/Next/Tailwind 组件代码** + 预览 + 社区 themes + **SVG logo(导 JSX/TSX/SVG)**。这是本清单里唯一"代码级"结构化数据。
- **覆盖/规模**:大(sitemap 9696;community 组件按 hero/features/pricing/testimonials 等 30+ 类)。
- **付费墙**:**浏览 + Magic MCP 免费计划就有**(10 credits/mo,无限 UI inspirations + 无限 SVG logo);Pro $20、Pro Plus $40/mo 仅加生成额度。
- **程序化**:**最强**——官方 **llms.txt(8.7KB,结构完整)** + Magic MCP(`21st.dev/api/mcp`,需 free API key,x-api-key/Bearer)+ api.21st.dev + robots **明确 Allow ClaudeBot `/community/`** + sitemap 9696。组件走 shadcn 风 registry。
- **结论**:**值得归档 / 或直接 MCP,取决于用途**。若要"离线组件弹药库"→ 归档 community 组件的代码 + 元数据(有稳定组件页 + registry 模式,可批量);若只需即时生成→ 挂 Magic MCP。**这是继 refero styles 之后最该动的一个**,因为它是**代码(可直接用),不是截图(只能看)**。红线:组件多为社区 UGC,注意各组件 license(publish 页允许 monetize,说明有授权层)。
- 来源:llms.txt(实测全文);robots.txt(ClaudeBot Allow /community/);sitemap(9696);api/mcp 实测 401 需 key

### uiverse.io — uiverse.io
- **内容类型**:**开源 UI 组件(纯 HTML/CSS + Tailwind)**,可直接复制代码。button/card/loader/toggle 等小组件为主。
- **规模**:大(社区海量小组件)。
- **付费墙**:**全免费 + 开源(MIT/CC)**,主打"free & open-source"。
- **程序化**:robots 极宽松(仅 Disallow `/admin`)。SPA,未见公开官方 API/llms.txt,但**内容是纯 HTML/CSS 代码,DOM 里就有可提取**;爬取门槛低(无 Cloudflare 挡)。
- **结论**:**值得归档(轻量)**。它是"CSS 微组件"弹药——button/loader/checkbox 等原子级样式,agent 做前端时能直接贴。价值定位与 21st(整段 section)互补:**uiverse=原子组件,21st=业务 section**。归档路径:爬组件详情页提 HTML/CSS + tag。授权友好(开源)是最大加分。
- 来源:robots.txt(实测,仅禁 /admin);站点定位"free open-source UI"

### Component Gallery — component.gallery
- **内容类型**:**设计系统组件的"跨库对照"索引**(同一组件如 Accordion,列出各大设计系统的实现 + 文档链接)。是"组件百科/参考",非可复制代码本身。
- **规模**:中(按组件类型编目,链到 Material/Carbon/Polaris 等)。
- **付费墙**:无。
- **程序化**:robots Cloudflare Content-Signal(`ai-train=no, use=reference`);内容是结构化编目(组件名 × 各设计系统实现),**结构清晰但价值是"链接聚合"**。
- **结论**:**不值得整库归档**,但**它的"组件→各设计系统"映射表**对 agent 有工具书价值(想知道"Accordion 在 Carbon 里怎么叫/长啥样"→查它)。轻量抓一份索引即可,不必全量。
- 来源:robots.txt(实测 Content-Signal)

---

## D 组(自行发现:2025-2026 对 AI 友好的新形态)

**趋势判断**:比起"爬第三方截图库",2025-2026 更贴近 refero-价值的是「**设计源头/规范直接暴露 MCP 或结构化数据**」。

- **Figma 原生 MCP**(2025 末上线):选中图层 → 直接发 **CSS + 字体引用 + 色板 + 截图** 给模型。这是"design tokens 喂 agent"的官方通道,**比爬任何截图库都权威**。若用户的设计源在 Figma,这是第一优先。(来源:WebSearch,Figma MCP 2025 末;**未逐字验证官方文档**)
- **"设计规范即 MCP"**:如 **HIG Doctor**(Apple Human Interface Guidelines 审计 CLI + MCP,覆盖 SwiftUI/React/Next/Flutter/HTML/CSS),给 agent 查设计准则 + 审计项目。这类"规范库"是 refero-tokens 的规则版。(来源:WebSearch awesome-ai-agents-2026;**未逐一验证**)
- **shadcn / Tailwind registry 生态**:21st.dev、uiverse 都走"组件 registry"模式,是当前"组件即数据"的主流载体。
- **llms.txt 采用面**:本次清单里仅 **21st.dev、Mobbin** 有 llms.txt;多数灵感库(recent/land-book/lapa/uiverse/saas)仍 404 —— 说明"截图类灵感库"整体还没把 agent 当一等公民,**代码/规范类才是 AI 友好前沿**。

---

## 总表(库 × 内容类型 × 可程序化 × 建议动作)

| 库 | 内容类型 | 结构化数据? | 付费/墙 | 程序化入口 | 反爬 | 建议动作 |
|---|---|---|---|---|---|---|
| **21st.dev** | React/Tailwind **代码**+SVG logo | ✅ 代码+registry | 浏览/MCP免费 | llms.txt+MCP+api+sitemap9696 | 无 | **归档(代码)或挂MCP** ⭐ |
| **uiverse.io** | HTML/CSS **微组件**(开源) | ✅ 代码(DOM内) | 全免费开源 | robots宽松,DOM可提 | 无 | **轻量归档(原子组件)** ⭐ |
| **SiteInspire** | 网站截图+taxonomy | ✖ 仅截图URL | 免费 | **官方MCP(匿名可调)** | 无 | **挂MCP实时查**(A档标杆) |
| **Mobbin** | app/web截图+flows | ✖ 仅截图 | 极严墙 | MCP(**仅付费**)+llms.txt | GPTBot禁 | 有付费账号→MCP;否则喂截图 |
| **Recent(=Godly)** | 视频/图片流 | △ 稳定id+CDN直链,无token | 免费无墙 | api.recent.design(路由未验)+CDN | 无(ai-train=no) | 半值:可归档截图做reference,禁训练 |
| **SaaS Landing Page** | 落地页截图+文章 | △ 文章文本 | 免费 | WP/Yoast sitemap | 无(ai-train=no) | 边缘:SaaS方向可选爬文本 |
| **component.gallery** | 组件跨库对照索引 | △ 编目映射 | 免费 | Cloudflare content-signal | 软 | 轻量抓索引当工具书 |
| **Page Flows** | **录屏视频**+截图 | ✖ 视频 | 全付费 | sitemap空 | 登录墙 | 人看流程→喂关键帧 |
| **Land-book** | 落地页截图 | ✖ 仅截图 | 部分Pro | robots禁/api+禁参数 | **Cloudflare硬** | 人逛→喂截图 |
| **Lapa Ninja** | 落地页截图+免费素材 | ✖ 仅截图 | 大量免费 | 403挡 | **Cloudflare硬** | 人逛/浏览器单取 |
| **Awwwards** | 获奖网站截图 | ✖ 仅截图 | 混合 | sitemap非XML | 软 | 看趋势→喂截图 |
| **Collect UI** | Dribbble截图聚合 | ✖ 仅截图 | 免费 | 无API | 软 | 人逛→喂截图 |
| **Minimal Gallery** | 极简网站截图 | ✖ 仅截图 | 免费 | 无 | 无 | 人逛(样本小) |
| **Dark Mode Design** | 暗色网站截图 | ✖ 仅截图 | 免费 | 无(站点探查失败) | ? | 人逛(稳定性存疑) |
| **Curated.design** | 素材/资源导购 | ✖ 外链合集 | 混合 | sitemap有但非设计数据 | 无 | 找素材时人逛 |
| **Refero本体** | UI/UX截图库 | ✖ 本体仅截图 | 有登录 | SPA壳,无公开API | 软 | 本体不归档(tokens在styles子站,已做) |

图例:✅=可直接用的结构化数据(代码/tokens);△=部分结构化(文本/id/映射);✖=纯截图/视频。

---

## 下一个最值得归档的 2-3 个库(推荐 + 理由)

**#1 — 21st.dev(强烈推荐,最像 refero 的"升级版")**
理由:它是全清单里**唯一提供"可直接复制的业务级组件代码"(React/Next/Tailwind)+ 结构化 registry + 完整 llms.txt + sitemap 9696**的库。refero styles 给的是"风格 tokens",21st 给的是"成品组件代码"——对 agent 写前端是**从'知道该长啥样'升级到'直接产出可用代码'**。免费计划即可用,ClaudeBot 白名单,授权层清晰(publish 允许 monetize)。路径:① 短期直接挂 Magic MCP(申请 free key,一行接入,即时生成);② 若要离线弹药库,按 community 组件页 + registry 批量爬代码 + 元数据(hero/pricing/testimonials 等分类做检索维度),做成"组件 skill"。

**#2 — uiverse.io(推荐,互补且授权最干净)**
理由:**纯 HTML/CSS 原子微组件(button/loader/toggle/checkbox)+ 全开源(MIT/CC)+ 无 Cloudflare 挡**。与 21st 互补(uiverse=原子样式,21st=整段 section)。开源授权 = 归档零法律风险,这是它压过所有截图库的关键。路径:爬组件详情页提 HTML/CSS + tag,做成"CSS 微组件 skill",agent 做交互细节时直接贴。规模大、门槛低、可当第二批。

**#3(条件性)— SiteInspire,但"挂 MCP"而非"归档"**
理由:它有**匿名可调的官方 MCP**,是"网站级视觉参考"里 agent 接入成本最低的。**不建议归档**(数据是截图 URL,离线无意义),但**强烈建议直接把它的 MCP 挂进 agent**——一行配置,就让 agent 获得"按 style/type/subject 检索策展网站 + 拿截图"的能力,补上 refero(UI 组件)+ 21st(代码)之外的"整站视觉调性"维度。

**明确不建议动的**:Mobbin/Page Flows(付费墙+反爬,ROI 低)、Land-book/Lapa(Cloudflare 硬挡)、Awwwards/Collect UI/Minimal/Dark Mode(纯截图,归档=一堆离线图,不如需要时喂)。

---

## 未验证 / 风险标注
- **recent.design 的 `api.recent.design` 精确路由未提取到**(serverFn 需 HTML 上下文,根 404);其 robots `ai-train=no` 是**训练红线**——只可 reference 用途。
- **Figma MCP / HIG Doctor** 来自 WebSearch,**未逐字核对官方文档**。
- Lapa 镜像 `no-ads--lapaninja.netlify.app` **未验证**官方性/时效。
- 各库 ToS 仅据 robots + 公开说明做**简要**判断,归档前应逐库核对 Terms(尤其 21st 组件的逐个 license、Mobbin/Recent 的 ai-train 条款)。
- Dark Mode Design 站点探查连接失败(000),在线状态存疑。
- 付费墙具体额度(Mobbin 4app/10图、Refero 登录门槛)取自搜索+页面信号,数字可能随时间变动。
