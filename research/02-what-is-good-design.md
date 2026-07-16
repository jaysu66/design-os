# 什么样的网页/交互设计是"好"的 —— 给 AI coding agent 学习的判别标准体系

> 用途：为 Codex/Claude 生成"高级感网页"的能力库提供**可判别、有出处、可执行**的"什么是好"锚点。
> 编写日期：2026-07-07。所有论点带来源；无一手确证的标注「未验证」。
> 读法：每节末尾的「agent 可执行规则」是把认知压缩成的 if-then / 数值范围，直接可写进 prompt 或 lint 规则。

---

## 第 1 节：业界评价体系（"好"的行业共识锚点）

### 1.1 Awwwards —— 四维加权评分（行业最通用的锚点）

四个评分维度及**官方权重**（来自 Awwwards 官方 Evaluation 页）：

| 维度 | 权重 | 含义 |
|------|------|------|
| **Design（设计）** | **40%** | 视觉执行质量：排版、色彩、布局、图像、整体视觉打磨度 |
| **Usability（易用性）** | **30%** | 可用性/交互流畅度、导航清晰、响应、功能可靠 |
| **Creativity（创意）** | **20%** | 原创性、概念新意、与同类的差异化 |
| **Content（内容）** | **10%** | 内容质量与相关性 |

关键推论：**Design + Usability = 70%**。即"好"的行业共识里，扎实的视觉执行 + 可用性远重于炫技创意（创意仅 20%）。这直接反驳"高级感 = 堆特效"。

评审机制（官方 + 一手）：
- 每个站点送至**最少 18 名评委**；系统自动**剔除偏离均值最远的 3 个评分**（抗极端分）。投票期 5 天。
- **Honorable Mention（荣誉提名）**：均分 ≥ **6.5**。
- **Site of the Day (SOTD)**：当日最高分；可在满足高分 + ≥10 个 PRO 用户投票时提前颁出。
- **Developer Award**：SOTD 站点在开发者维度评分 > **7.0**。
- **SOTM（月）**：当月 8 个最高分复评；**SOTY（年）**：SOTM 优胜者 + 精选，次年 2 月公布。

来源：[Awwwards Evaluation System](https://www.awwwards.com/about-evaluation/)、[webdesignawards.io Awwwards review](https://www.webdesignawards.io/awards/awwwards)

### 1.2 CSS Design Awards (CSSDA) —— 更偏"产品/UX 工艺"

三维加权：**UI 40% / UX 30% / Innovation 30%**。对比 Awwwards：把"Design"细分为 UI+UX，创新权重更高（30% vs 20%），更适合评判**产品型/UX 主导**站点，而非营销视觉。
来源：[webdesignawards.io compare](https://www.webdesignawards.io/compare/awwwards-vs-cssda-vs-fwa-vs-web-design-awards)、[Utsubo judging criteria](https://www.utsubo.com/blog/award-winning-website-design-guide)

### 1.3 FWA (Favourite Website Awards) —— 偏实验/前卫

无公开加权评分表；导向为**创新 + 前卫 + 沉浸**（WebGL、AR/VR、motion-heavy launch）。500 人国际评委，日/月/年奖 + People's Choice。要做"技术炫技/沉浸叙事"看 FWA；要做"可用产品"看 CSSDA；要做"视觉品牌叙事"看 Awwwards。
来源：[webdesignawards.io compare](https://www.webdesignawards.io/compare/awwwards-vs-cssda-vs-fwa-vs-web-design-awards)

> **本节 agent 可执行规则**
> - IF 目标是"高级感/获奖级" → 优先把预算投在 **视觉执行(40%) + 可用性(30%)**，创意特效只占 20%，不要本末倒置。
> - 视觉打磨的判别底线可锚定 Awwwards 荣誉线 6.5/10：排版对齐、色彩克制、层级清晰、无廉价默认样式。
> - IF 产品型 UI（dashboard/SaaS）→ 用 CSSDA 心智：UI+UX 工艺 > 创意；状态完整、交互可靠优先。
> - IF 营销/品牌落地页 → 允许更多创意与 motion，但仍以视觉执行为骨。

---

## 第 2 节：顶级产品设计语言拆解（提炼成 agent 可执行模式）

### 2.1 Linear —— "以工艺(craft)取胜 + 强观点(opinionated)"

Karri Saarinen（Linear CEO/设计出身）**10 条打磨产品的规则**（Figma 官方博客）中，对 agent 直接可用的：
- **规则 5：把 spec 当"最低可行下限"，不是终点**——"团队要把 spec 看成 baseline，不是 finish line"。→ 生成 UI 时不要止步于"功能能用"，要追加打磨层（状态、微交互、间距节奏）。
- **规则 7：最好的设计是有观点的（opinionated）**——"为某个具体的人设计，而不是所有人"。→ 拒绝"什么都行"的通用模板，做出明确的默认与立场。
- **规则 8：提升质量最简单的方式是减少 scope**——少做，但做到极致。→ 一个页面 3 个核心信息，别塞满。
- **规则 6：质量≠完美**——达到质量 bar 就发，再迭代。
- **规则 10：数据是拐杖**——"要给最好体验，你得让用户惊喜；数据不会告诉你怎么做"。→ 签名时刻/惊喜细节靠品味，不靠 A/B。

Linear 内部对"品味(taste)"的定义（招聘四维之一）："**识别质量、在意工艺的能力**……能清楚说出什么让工具变好或变坏"。这就是 agent 要习得的判别力本身。
来源：[Figma Blog — Karri Saarinen's 10 Rules](https://www.figma.com/blog/karri-saarinens-10-rules-for-crafting-products-that-stand-out/)、[Sequoia — Linear Spotlight](https://sequoiacap.com/article/linear-spotlight/)、[techinterview — Linear taste dimension](https://www.techinterview.org/companies/linear/)

**Linear 视觉签名（第三方拆解，标注：部分未逐一官方确证）**：极暗中性底色 + 高饱和单一强调色（紫/蓝）、细 1px 边框而非重阴影、克制的密度、快到"零延迟感"的交互（速度即产品哲学）。

### 2.2 Vercel Geist —— "极致高对比 + 负字距大标题 + 严格间距刻度"

来自 Geist 官方文档 + 多方拆解：
- **字体系统**：Geist Sans（正文）+ Geist Mono（等宽，常用于标题/技术感）。
- **大标题（48–64px）**：`letter-spacing: -0.04em` + `line-height ≈ 1.15`——负字距 + 紧行高造出"凝练高冲击"标题。这是 Geist 标志性观感。
- **色彩系统（业界最"stark"之一）**：近白底 `#fafafa`、近黑墨 `#171717`、以及一整套**200 步灰阶**——每条分割线/边框/disabled 态都落在**刻意的某一级灰**上，不随手取色。
- **间距刻度**：`[4, 8, 12, 16, 24, 32, 40, 48, 64]`。
- **圆角**：UI 元素锐利 6px + 药丸按钮 9999px 混用；**深度靠 1px 细边框 + 极轻 box-shadow**，不靠重投影。
- **排版哲学一句话**："文字很密，文字周围的空间很大"（dense text, vast surrounding whitespace）——负字距的紧凑被大量留白反向平衡。
- Typography 以 Tailwind class 暴露（如 `text-heading-72`、`text-copy-14`，**类名即含 px**），每个 class 预设 font-size/line-height/letter-spacing/font-weight 组合。

来源：[Vercel Geist Introduction](https://vercel.com/geist/introduction)、[Vercel Geist Typography](https://vercel.com/geist/typography)、[SeedFlip Vercel breakdown](https://seedflip.co/blog/vercel-design-system)、[designsystems.surf Geist](https://designsystems.surf/design-systems/vercel)

### 2.3 Stripe —— "为性能而生的克制动效 + 拟人化微交互"

Stripe 工程/设计博客与技术拆解：
- **只动 transform（translate/scale/rotate）+ opacity，绝不动 layout 属性（width/height/top/left）**——后者触发 layout/paint，掉帧。这是 60/120fps 丝滑的底层铁律。
- **把工作移出主线程**：微交互元素提升到**独立合成层**，用 transform+opacity 做到每帧 <10ms。
- **Web Animations API 作默认**：需要可交互/可链式/随机效果时用 WAAPI，而非纯声明式。
- **拟人化/skeuomorphic 微动效**：出错时轻微 3D 旋转，"像人被指出错误时摇头"——在潜意识层加"温度"。→ 动效是为沟通状态与情感，不是装饰。
- 设计-工程协作前置："设计师选**在性能预算内可实现**的动效"——设计决策先受性能约束。

来源：[Stripe Blog — Connect front-end experience](https://stripe.com/blog/connect-front-end-experience)、[Stripe Blog — interactive globe](https://stripe.com/blog/globe)、[Improve payment experience with animations (Michaël Villar)](https://medium.com/bridge-collection/improve-the-payment-experience-with-animations-3d1b0a9b810e)

### 2.4 Apple（HIG）—— "真实可信 + 快而精确"的动效观

- 动效要**有意图**：让人保持定位、对操作给清晰反馈、帮人学界面而不淹没。
- **追求真实可信（realism/credibility）**：违反物理直觉的动效令人迷失。
- **偏好快而精确的动画**："brevity + precision → 更轻、更不打扰、更有效传达信息"。
- **Reduce Motion**：缩放/旋转/外围运动可致眩晕，须可关闭或提供替代动画。

来源：[Apple HIG — Motion](https://developer.apple.com/design/human-interface-guidelines/motion)

> **本节 agent 可执行规则（把 5 家压成模式）**
> - **色彩策略**：默认中性底 + 单一高饱和强调色（Linear/Vercel）。近黑墨 `#171717`、近白底 `#fafafa` 是安全高级组合。避免"到处渐变、到处紫"。
> - **大标题**：字号 ≥48px 时加负字距 `letter-spacing: -0.02em ~ -0.04em`，行高收紧到 1.05–1.2（Geist 模式）。
> - **深度**：优先 `1px border`（可用强调色 20% 透明度）+ 极轻阴影表达层级，而非 `shadow-lg` 重投影（Vercel）。
> - **间距**：锁一套刻度 `[4,8,12,16,24,32,40,48,64]`，全站只从里面取值。
> - **动效铁律**：只 animate `transform` + `opacity`；禁 animate `width/height/top/left`（Stripe）。目标 60fps。
> - **动效目的**：动效服务于"沟通状态/反馈/情感"，快而精确（Apple/Stripe）；高频操作（菜单/切换）尽量不加入场动画（见第 4 节 Rauno）。
> - **观点化**：给出明确默认，不做"什么都行"的模板（Linear 规则 7）。
> - **打磨层**：功能可用后追加 状态完整 + 微交互 + 间距节奏，把 spec 当下限（Linear 规则 5）。

---

## 第 3 节：经典理论 → 网页落地

### 3.1 视觉层级：四杠杆（size / weight / color-value / space）

强层级 = **在每个层级上叠加 2–3 个杠杆**，而非只靠一个：
- **Size/Scale**：更大 = 更重要；把标题拉到远高于正文是"最直接的层级信号"。
- **Weight**：粗体前进、细体后退；可在不改字号时分层。
- **Color/Value**：高饱和/高对比跳到前景，灰淡后退。
- **Space**：元素周围留白多 = 被"提升"；紧凑分组 = 下级支撑信息。

来源：[UX Pilot — Visual Hierarchy](https://uxpilot.ai/blogs/visual-hierarchy)、[IxDF — Visual Hierarchy](https://ixdf.org/literature/topics/visual-hierarchy)、[Toptal — Visual Hierarchy](https://www.toptal.com/designers/ux/visual-hierarchy-infographic)

### 3.2 Type scale（模块化比例）

每级 = 下一级 × 固定比例。常用比：**Minor Third 1.2（微对比）/ Major Third 1.25（中等）/ Golden 1.618（戏剧化）**。
示例：16px 起、1.25 比 → 16, 20, 25, 31, 39, 49px。
来源：[Made Good Designs — Hierarchy](https://madegooddesigns.com/hierarchy-in-design/)、[IxDF — Visual Hierarchy](https://ixdf.org/literature/topics/visual-hierarchy)

### 3.3 8pt 网格与间距体系

- 用 8 的倍数（8,16,24,32,40,48,56…）做 padding/margin/尺寸——数学简单、跨端一致。
- 常配 **4pt 基线网格**处理更细的文字对齐（8pt UI + 4pt baseline）。
- 间距原则："**内部间距 ≤ 外部间距**"（元素内部 padding 应小于元素之间的间隔，强化分组）。
来源：[Cieden — Spacing best practices](https://cieden.com/book/sub-atomic/spacing/spacing-best-practices)、[UX Planet — 8pt grid](https://uxplanet.org/everything-you-should-know-about-8-point-grid-system-in-ux-design-b69cb945b18d)、[Designsystems.com — Space grids layouts](https://www.designsystems.com/space-grids-and-layouts/)

### 3.4 格式塔原则在 UI 的落地

- **Proximity（邻近）**：靠得近 = 被视为一组 → 相关信息聚拢、无关拉开（省去分割线）。
- **Common Region（共同区域）**：同一边界/卡片内 = 一组 → 卡片、分组容器。
- **Similarity（相似）**：同色/同形/同尺寸 = 同类；辨识优先级 **颜色 > 尺寸 > 形状**。
- **Closure（闭合）**：露出局部即可让人脑补整体 → "露半个卡片"暗示可滚动。
- **Figure/Ground、Continuation、Prägnanz（简约对称）**同属六大原则。
来源：[NN/G — Common Region](https://www.nngroup.com/articles/common-region/)、[NN/G — Proximity](https://www.nngroup.com/articles/gestalt-proximity/)、[IxDF — Gestalt Principles](https://ixdf.org/literature/topics/gestalt-principles)

### 3.5 色彩系统方法

- **60-30-10**：主色/背景 60% + 辅助色 30% + 强调/CTA 10%，天然平衡。
- **OKLCH（现代趋势）**：CSS Color 4 的 `oklch(L C H)`，**感知均匀**（改 L 只改亮度、不偏色），比 HSL/sRGB 更宽色域、渐变更干净、明暗态调节更直觉。做设计 token / 一致的明暗阶推荐 OKLCH。
来源：[sixtythirtyten — 60-30-10](https://www.sixtythirtyten.co/blog/60-30-10-rule-complete-guide)、[CSS-Tricks — oklch()](https://css-tricks.com/almanac/functions/o/oklch/)、[MDN — Color space](https://developer.mozilla.org/en-US/docs/Glossary/Color_space)

### 3.6 动效原则与公认参数（可直接抄的数值）

**Material Design 3 —— 精确 token（GitHub 官方源，可直接用）：**

Easing（cubic-bezier）：
- Standard：`cubic-bezier(0.2, 0, 0, 1)`（最常用，起止皆静止的元素）
- Standard Decelerate（入场）：`cubic-bezier(0, 0, 0, 1)`
- Standard Accelerate（出场）：`cubic-bezier(0.3, 0, 1, 1)`
- Emphasized Decelerate：`cubic-bezier(0.05, 0.7, 0.1, 1)`
- Emphasized Accelerate：`cubic-bezier(0.3, 0, 0.8, 0.15)`
- Linear：`cubic-bezier(0, 0, 1, 1)`（仅用于 fade/持续性运动）

Duration（ms）：short1-4 = **50/100/150/200**；medium1-4 = **250/300/350/400**；long1-4 = **450/500/550/600**；extra-long = 700–1000。
用法：**小元素/简单变化用短时长（100–200ms）；大区域/复杂过渡用中长时长（300–500ms）**。**入场用 decelerate、出场用 accelerate**。

来源：[material-components-android Motion.md](https://github.com/material-components/material-components-android/blob/master/docs/theming/Motion.md)、[Material Design 3 — easing & duration](https://m3.material.io/styles/motion/easing-and-duration/tokens-specs)

**跨来源公认区间**：桌面动画 **150–200ms**、移动 **200–300ms**、可穿戴再短 ~30%。
来源：[Appy Pie — 200ms rule](https://www.appypie.com/blog/mobile-app-animation-guide)、[Material 1 — duration & easing](https://m1.material.io/motion/duration-easing.html)

**Disney 12 原则 → UI 映射（标注：映射为业界通行解读，非单一权威官方文档，部分未逐一确证）**：
- Slow In & Slow Out → 用 ease-in-out，别用 linear（除 fade）。
- Squash & Stretch → 按压时轻微 scale 反馈。
- Anticipation → 展开前的微预动作。
- Follow Through / Overlapping → 弹簧回弹、列表元素错峰入场（stagger）。
- Timing → 见上方 ms 区间。
（此处作为灵感清单，落地数值以 Material/HIG 为准。）

> **本节 agent 可执行规则**
> - 正文 **16–18px，行高 1.5–1.7**，测量宽度 **45–75 字符/行**（可读性通行区间，标注：行宽为通行经验值）。
> - Type scale 用固定比 **1.2 或 1.25**；标题字号随层级按比递增，别随手拍。
> - 间距只从 **8pt 刻度**取值；遵守"内部 padding < 元素间距"。
> - 分组优先用**邻近 + 共同区域**（留白/卡片），少用分割线。
> - 色彩用 **60-30-10** 配比；能用 **OKLCH** 定义 token 就用，明暗阶更干净。
> - 动效：默认 easing `cubic-bezier(0.2,0,0,1)`；时长小元素 **150–200ms**、大过渡 **300–400ms**；入场 decelerate、出场 accelerate；hover/淡入可用 linear。
> - 绝不 linear 做位移类动画（除纯 fade）。

---

## 第 4 节：交互质量标准

### 4.1 Nielsen 十大启发式（挑对视觉/交互设计最相关的 5 条）

1. **#1 可见的系统状态（Visibility of System Status）**：任何操作都要在合理时间内给反馈；用户知道当前状态才建立信任。→ loading/进度/成功态必给。
2. **#2 贴合现实世界**：用用户熟悉的概念/词汇/隐喻（Rauno 也强调"复用隐喻"）。
3. **#4 一致与标准**：同一词/操作在各处含义一致；遵循平台惯例（如 X 关闭、放大镜=搜索）。
4. **#6 识别优于回忆**：把选项/信息**可见**，别让用户记；减少记忆负担。
5. **#8 美学与极简设计**："消除会分散注意的无关信息……用极简 + 清晰层级"——这条直接对应"高级感靠克制"。
（其余 5 条 #3 用户控制/#5 错误预防/#7 灵活高效/#9 错误恢复/#10 帮助文档，偏功能可用性。）
1990 年 Nielsen & Molich 提出，1994 年经 249 个可用性问题因子分析精炼，**至今 10 条未变**。
来源：[NN/G — 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)、[NN/G — Visibility of System Status](https://www.nngroup.com/articles/visibility-system-status/)

### 4.2 响应时间阈值（硬数字，可直接编码）

- **0.1s（100ms）**：感觉"瞬时反应"的上限——此内无需特殊反馈，直接出结果。
- **1.0s**：思维流不被打断的上限——用户会察觉延迟但不烦。
- **10s**：注意力停留的上限——超过要给进度/预计完成反馈，允许用户干别的。
- **Doherty Threshold = 400ms**（IBM 1982）：系统响应 <400ms 时人机不互相等待、生产力飙升。
- 分档实践：打字/光标 <50ms 才觉实时；简单命令 100–400ms（Doherty 区）；系统操作 1–2s 配视觉反馈；复杂计算 2–10s 配进度条。
- **乐观 UI（Optimistic UI）**：先本地即时反馈、后台异步跑，可让"感知响应 <100ms"即便实际 500–1000ms（Gmail 发信即显）。

来源：[NN/G — Response Times: 3 Important Limits](https://www.nngroup.com/articles/response-times-3-important-limits/)、[Laws of UX — Doherty Threshold](https://lawsofux.com/doherty-threshold/)、[uxuiprinciples — Response Time Limits](https://uxuiprinciples.com/en/principles/response-time-limits)

### 4.3 界面状态完整性（"好"的重要判别：状态齐不齐）

每个交互元素至少覆盖：**enabled / hover / focus / active(pressed) / disabled**；数据视图另加 **loading / error / empty**（+ skeleton 首屏骨架）。
- **focus 态是无障碍必需**（键盘 Tab 导航需可见焦点）——常被 AI 生成漏掉。
- **disabled**：muted 填充 + 降透明度 + `cursor: not-allowed`，但保持可见（让用户知道"存在但暂不可用"）。
- **loading/error/empty 三态**：防止用户以为界面卡死、设定预期。空态 skeleton 优于纯 spinner（感知更快）。
来源：[NN/G — Button States](https://www.nngroup.com/articles/button-states-communicate-interaction/)、[Figma — Button States](https://www.figma.com/resource-library/button-states/)、[LogRocket — loading/error/empty states](https://blog.logrocket.com/ux-design/designing-instant-feedback-doherty-threshold/)、[Onething — Skeleton vs Spinner](https://www.onething.design/post/skeleton-screens-vs-loading-spinners)

### 4.4 Rauno Freiberg《Invisible Details of Interaction Design》(Vercel staff design engineer 一手)

- **高频交互不加入场动画**：命令菜单/右键菜单/应用切换器每天用几百次，动画变认知负担而非愉悦；改用**极短 blink/fade**确认。
- **手势即时响应**：pinch/drag 立即施加 scale/position delta，达阈值才触发动画（而非提前）。
- **轻/重操作区别对待**：轻量动作（overlay/搜索）滑动过程中即触发；破坏性动作（关闭/删除）留到手势结束才触发（防误伤）。
- **物理动量**：手势保留抛出的动量与角度，"从不完美居中或时序一致"；弹簧/减速曲线。
- **Fitts's Law**："目标越大、离指针越近越好"；高频动作放屏幕边缘（无限大命中区）。
- **可打断**：交互随时可中断，别强制播完动画。
- **不劫持滚动**：指针移到未聚焦窗口时别抢滚动事件。
（原文未给具体 ms/弹簧常数/命中尺寸数值。）
来源：[Rauno — Invisible Details of Interaction Design](https://rauno.me/craft/interaction-design)、[every.to 转载](https://every.to/p/invisible-details-of-interaction-design)

### 4.5 无障碍硬标准（WCAG + reduced motion）

- **对比度**：正文 ≥ **4.5:1**（AA）；大字（≥18pt 或 14pt 粗）≥ **3:1**；图标/UI 组件边界 ≥ **3:1**；AAA 正文 ≥ 7:1。
- **prefers-reduced-motion**：缩放/旋转/外围大幅运动会致眩晕；须 `@media (prefers-reduced-motion: reduce)` 提供关闭/替代（Apple HIG 同调）。
来源：[W3C WAI — Contrast Minimum 1.4.3](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum)、[W3C — G207 icon 3:1](https://www.w3.org/WAI/WCAG21/Techniques/general/G207)、[Apple HIG — Motion (Reduce Motion)](https://developer.apple.com/design/human-interface-guidelines/motion)

> **本节 agent 可执行规则**
> - 任何操作 IF 无法在 **100ms** 内出结果 → 给即时反馈（乐观 UI / spinner / 进度）；>1s 用进度条；>10s 给预计时间。
> - 交互目标响应窗口锚 **Doherty 400ms**。
> - 每个交互元素 MUST 有 hover + focus + active + disabled；数据区 MUST 有 loading + error + empty。**focus 态必须可见**（勿 `outline:none` 不补替代）。
> - 高频菜单/切换器**不加入场动画**（Rauno）；仅用 <150ms fade/blink 确认。
> - 正文对比度 ≥ **4.5:1**、大字/图标 ≥ **3:1**（可自动校验，命中即拒）。
> - MUST 加 `@media (prefers-reduced-motion: reduce)` 降级动画。
> - disabled = 降透明度 + `cursor:not-allowed`，仍可见。

---

## 第 5 节："高级感 / premium feel" 的可操作构成

核心命题（多来源共识）：**"AI 感"不是 AI 的错，是 default 的错**——LLM 放大训练数据里的默认写法（"AI look isn't about AI, it's about defaults"）。distinctive 的设计活在分布的长尾，模型系统性地丢掉长尾、回归"谁都不得罪的安全均值"。
来源：[dev.to (Alan West) — Fix the AI look](https://dev.to/alanwest/how-to-fix-the-ai-generated-look-in-your-frontend-1ahh)、[Medium (Samith) — Why AI UI feels generic](https://medium.com/@cssamithpitigala/why-ai-generated-ui-looks-good-but-often-feels-generic-020a9b1b8492)、[superdesign — Why AI design looks generic](https://superdesign.dev/blog/why-ai-design-looks-generic)

### 5.1 "AI 感"的可识别指纹（要主动规避的 tells）

- 默认 Tailwind 紫/靛/violet：`bg-indigo-600 hover:bg-indigo-700`、`slate-900`——"模型拿不定色就抓 indigo-600"。
- 千篇一律纵向堆叠：hero → features grid → social proof → pricing → FAQ → footer（不是因为对，是抄组件库示例）。
- Inter 打天下 + 偶尔 font-bold + 默认 line-height + 层级不清——"技术上可读，但彻底 forgettable"。
- 到处 `rounded-2xl shadow-lg p-6`（无圆角词汇、无意图）。
- 占位套话 copy："Empower your team…"、"Seamless Integration"、"Built for modern teams"——非技术者也会有"恐怖谷"反应。
- shadcn 默认 + 紫渐变 + Tailwind blue-500 + 同款字/同款圆角 = 一眼"AI 做的"。
来源：同上 Alan West / Samith / [DEV (samareshdas)](https://dev.to/samareshdas/why-most-ai-generated-websites-still-feel-generic-and-what-actually-makes-a-product-feel-premium-33p2)

### 5.2 高级感的构成要素（正向，可执行）

1. **意图性（intent）—— 最根本**："generic 与 great 的差别常是 intent：每个选择都有理由"（按钮大是因为它最重要；表单分步是因为用户会 overwhelmed；仪表盘只显 3 个指标是因为那 3 个最关键）。→ 每个视觉决策可追溯到一个理由。
2. **色彩克制 + 自定义**："别把 indigo 改名 primary，真去选不在默认 scale 里的颜色"；替换而非扩展调色板（默认色直接编译失败）。
3. **打破对称**：off-center 构图、重叠元素、内容突破网格 = "读起来像有意为之"；"居中 hero + 三卡 = 视觉上的米色地毯"。用 `grid-template-columns: minmax(2rem,1fr) minmax(0,38rem) minmax(0,1fr)` 造非对称。
4. **圆角/阴影词汇统一 + 克制**：全站选**一套圆角语言**（不是每组件一个）；去掉 `shadow-lg`，用 **1px border + 色彩对比**表达深度（呼应 Vercel/Linear）。
5. **真正的排版层级**：不是 Inter+bold，而是明确的 type scale + 字重/字距/行高组合（呼应第 3 节 + Geist 负字距）。
6. **签名时刻（signature moment）**：一处克制但独特的细节/微动效制造记忆点（Linear 规则 10"给用户惊喜"）——但只 1–2 处，不滥用。
7. **细节密度与自洽**："差别通常不是复杂度，是 attention to detail"；视觉节奏/section pacing/contrast balance/interaction feedback 全部一致 → 观感"intentional 而非 generated"。
8. **真实文案 + 具体数字**：至少一句"像真人写的"、至少一个带具体数字的断言；禁 Empower/Unlock/Transform 开头、禁抽象名词对（Seamless Integration）。
9. **真实动效**：加"真实的 motion"让它 feel premium——但遵守第 3 节性能与时长铁律。

来源：[Alan West — Fix the AI look](https://dev.to/alanwest/how-to-fix-the-ai-generated-look-in-your-frontend-1ahh)、[gendesigns — AI UI mistakes](https://gendesigns.ai/blog/ai-generated-ui-mistakes-how-to-fix)、[Superdesign — make AI UI less generic](https://superdesign.dev/blog/how-to-make-ai-ui-look-less-generic)、[DEV (samareshdas)](https://dev.to/samareshdas/why-most-ai-generated-websites-still-feel-generic-and-what-actually-makes-a-product-feel-premium-33p2)

> **本节 agent 可执行规则（可直接做成 lint / 生成前置约束）**
> - **BAN 默认色**：禁 `bg-(indigo|violet|purple)-600`、`slate-900` 兜底；MUST 定义自定义 token（近黑 `#171717` / 近白 `#fafafa` / 单一强调色）。可用 ESLint 正则拦截：`/bg-(indigo|violet|purple)-600/`、`/from-purple-\d+ to-(blue|pink)-\d+/`。
> - **BAN 万能圆角**：禁 `rounded-(2xl|3xl)` 到处用；全站锁 1 套圆角（如 6px + pill），阴影用 1px border 替代。
> - **BAN 套话文案**：禁 "Empower/Unlock/Transform/Seamless/Built for modern teams" 开头；MUST ≥1 具体数字 + ≥1 句"像真人写"。
> - **BAN 默认布局**：不要无脑 居中 hero + 三卡纵向堆叠；至少一处非对称/破格构图。
> - **字体**：禁"Inter+bold 打天下"；MUST 明确 type scale + 大标题负字距（-0.02~-0.04em）。
> - **签名时刻**：每页留 1–2 处克制的独特细节/微动效；其余克制。
> - **意图自检**：生成后逐项问"这个选择的理由是什么"，答不出 = 是 default，替换掉。

---

## 附：给 agent 的"一页判别清单"（把全文压成可勾选项）

视觉执行（占"好"的 40%，最重）
- [ ] 色彩克制：中性底 + 单一强调色；无默认紫/到处渐变
- [ ] 大标题负字距（≥48px 时 -0.02~-0.04em），行高 1.05–1.2
- [ ] 正文 16–18px / 行高 1.5–1.7 / 行宽 45–75 字符
- [ ] type scale 固定比 1.2 或 1.25
- [ ] 间距全走 8pt 刻度；内部 padding < 元素间距
- [ ] 深度靠 1px border（可 20% 透明强调色）而非重阴影
- [ ] 全站 1 套圆角词汇

可用性（占 30%）
- [ ] 每交互元素有 hover/focus/active/disabled；focus 可见
- [ ] 数据区有 loading/error/empty（首屏 skeleton）
- [ ] 响应 <100ms 即时反馈，>1s 进度条，>10s 预计时间（乐观 UI 优先）
- [ ] 对比度 正文≥4.5:1 / 大字·图标≥3:1
- [ ] `prefers-reduced-motion` 降级

动效（服务沟通，别炫技）
- [ ] 只 animate transform+opacity；禁 layout 属性
- [ ] 时长：小 150–200ms / 大 300–400ms；默认 easing cubic-bezier(0.2,0,0,1)
- [ ] 入场 decelerate、出场 accelerate；fade 才用 linear
- [ ] 高频菜单/切换器不加入场动画

高级感/去 AI 感
- [ ] 无 Empower/Seamless 套话；≥1 具体数字
- [ ] 至少 1 处非对称/破格构图
- [ ] 1–2 处克制签名细节
- [ ] 每个视觉决策答得出"为什么"（否则是 default，替换）

---
报告完。数值区间中标注"通行经验值/未逐一确证"的项（行宽 45–75、Disney 12 映射、Linear 视觉签名细节）为跨来源通行解读，非单一权威官方数字，落地以带精确来源的 Material/HIG/WCAG/Geist 官方值为准。
